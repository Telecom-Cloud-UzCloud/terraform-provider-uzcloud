# Changelog

[🇬🇧 English](CHANGELOG.md) | [🇷🇺 Русский](CHANGELOG.ru.md) | [🇺🇿 O'zbekcha](CHANGELOG.uz.md)

## Unreleased

## 1.1.0 (16 октября 2026)

##### НОВОЕ:

* storage: **Новый ресурс:** `uzc_object_storage`, объектное хранилище, совместимое с S3 (Ceph): создание, импорт, изменение `size_gb` и `billing_cycle` на месте, экспорт `s3_endpoint`
* kubernetes: `uzc_kubernetes_cluster` принимает `master_plan`, `worker_plan`, `blockstorage_plan` и `cloud_provider_setup`, как при создании кластера в консоли UzCloud; `plan` продолжает работать и используется как тариф воркеров
* provider: новая настройка `operation_timeout_minutes` (или `UZC_OPERATION_TIMEOUT`, по умолчанию 20), ограничивающая любое ожидание асинхронной операции

##### УЛУЧШЕНИЯ:

* provider: каждый ресурс дожидается, пока UzCloud подтвердит результат операции (созданный ресурс читается, изменение видно, удалённый ресурс исчез), и падает с явной ошибкой по истечении `operation_timeout_minutes`; у `uzc_virtual_machine` остаются собственные таймауты создания, изменения и удаления
* provider: ошибки API содержат детали валидации по полям, которые присылает UzCloud, например `[storage_category: The storage category field is required.]`, а не только «Validation errors»
* storage: `uzc_vm_snapshot`, `uzc_blockstorage_snapshot`, `uzc_vm_backup` и `uzc_blockstorage_backup` по умолчанию используют цикл оплаты `hourly`: для этих сервисов с оплатой по факту API отвергает месячный и годовой циклы
* storage: `uzc_blockstorage_attachment` ждёт, пока диск будет показан подключённым (и отключённым при удалении); снапшот диска повторяется, пока диск не станет Ready; удаление диска повторяется, пока ещё идёт отключение
* compute: ожидание публичного IP ограничено 10 минутами и завершается подсказкой про квоту IP-адресов; VM, ожидание которой не удалось, сохраняется в state как tainted, а не остаётся работать без управления
* compute: удаление VM повторяется, пока ещё отменяется её расписание бэкапов
* network: переименование `uzc_vpc`, `uzc_network`, `uzc_project` и `uzc_vpn_customer_gateway` проверяется повторным чтением ресурса
* tests: acceptance-тесты для каждой группы ресурсов, бесплатный смоук-тест всех датасорсов и тесты-разведчики, печатающие тарифы, облачные провайдеры, регионы и сырые ответы API аккаунта

##### ИСПРАВЛЕНИЯ:

* compute: создание `uzc_virtual_machine` падало с «Value Conversion Error» на `offering`/`ipaddresses`
* compute: запрос создания VM совпадает с консолью (`username`, `null` вместо пустых строк); переименование отправляет `vm_label`
* network: `uzc_vpc` отвергал CIDR с префиксом («The cidr field must be a valid IP address»); теперь принимаются и `10.0.0.0/16`, и `10.0.0.0`
* network: `region`, `project` и `billing_cycle`, унаследованные из блока провайдера, оставались неизвестными после создания `uzc_vpc`, `uzc_network`, `uzc_ip_address` и `uzc_virtual_machine`
* network: пустой ответ на изменение затирал slug у `uzc_vpc`, `uzc_network` и `uzc_vpn_customer_gateway`, после чего ломалось удаление
* project: создание `uzc_project` падало с 422 (теперь всегда отправляется `icon`, по умолчанию `cloud-15`, а `status` как 1/0); статус, пришедший строкой, разбирается; удаление идёт через `DELETE /projects/{slug}`
* provider: булевы поля в multipart-запросах отправляются как 1/0, как требует валидация Laravel (она отвергает «true»/«false»)
* provider: любой неожиданный ответ 4xx (400, 405, 409 и т. д.) считается ошибкой; раньше он считался успехом
* vpn: `uzc_vpn_customer_gateway` отправляет `dead_peer_detection` (написание из спецификации отвергалось) и сохраняет `gateway` при чтении; `uzc_vpn_user` находит свой slug после создания
* storage: `uzc_blockstorage` больше не требует пересоздания из-за того, что API не возвращает `storage_category` (он читается из `storage_setting`); `uzc_blockstorage_attachment` больше не выпадает из state при обновлении
* data sources: `uzc_plans` и `uzc_account_balance` принимают числа, присланные строкой, и атрибуты, присланные пустым массивом

##### ВНИМАНИЕ:

* provider: `terraform destroy` теперь ждёт фактического удаления ресурсов, а не завершается сразу после принятия заявки на отмену; удаление занимает больше времени и падает по истечении `operation_timeout_minutes`
* storage: снапшоты и бэкапы теперь по умолчанию используют `hourly`; если нужен другой цикл, задайте `billing_cycle` явно
* api: известные проблемы на стороне UzCloud, которые не исправить в провайдере: переименование VM падает (`chageLabel`), переименование VPC принимается, но не применяется, добавление SSH-ключа отвечает 500, удаление проекта отклоняется («проекты с услугами нельзя удалить»), а создание Kubernetes на тестовом аккаунте отвечает «not configured properly»

## 1.0.2 (4 сентября 2026)

##### НОВОЕ:

* provider: первый релиз Terraform-провайдера UzCloud, построенного на Terraform Plugin Framework
* project: **Новый ресурс:** `uzc_project`
* project: **Новый датасорс:** `uzc_projects`
* project: **Новый датасорс:** `uzc_project`

* compute: **Новый ресурс:** `uzc_virtual_machine`
* compute: **Новый ресурс:** `uzc_ssh_key`
* compute: **Новый ресурс:** `uzc_template`
* compute: **Новый ресурс:** `uzc_iso`
* compute: **Новый ресурс:** `uzc_vm_snapshot`
* compute: **Новый ресурс:** `uzc_vm_snapshot_revert`
* compute: **Новый ресурс:** `uzc_vm_backup`
* compute: **Новый ресурс:** `uzc_affinity_group`
* compute: **Новый датасорс:** `uzc_ssh_keys`
* compute: **Новый датасорс:** `uzc_templates`
* compute: **Новый датасорс:** `uzc_account_templates`
* compute: **Новый датасорс:** `uzc_marketplace_apps`
* compute: **Новый датасорс:** `uzc_isos`
* compute: **Новый датасорс:** `uzc_vm_snapshots`
* compute: **Новый датасорс:** `uzc_affinity_groups`

* storage: **Новый ресурс:** `uzc_blockstorage`
* storage: **Новый ресурс:** `uzc_blockstorage_attachment`
* storage: **Новый ресурс:** `uzc_blockstorage_snapshot`
* storage: **Новый ресурс:** `uzc_blockstorage_snapshot_revert`
* storage: **Новый ресурс:** `uzc_blockstorage_backup`
* storage: **Новый датасорс:** `uzc_blockstorages`
* storage: **Новый датасорс:** `uzc_blockstorage`
* storage: **Новый датасорс:** `uzc_blockstorage_snapshots`

* network: **Новый ресурс:** `uzc_network`
* network: **Новый ресурс:** `uzc_vpc`
* network: **Новый ресурс:** `uzc_vpc_restart`
* network: **Новый ресурс:** `uzc_vpc_acl_rule`
* network: **Новый ресурс:** `uzc_virtual_router`
* network: **Новый ресурс:** `uzc_ip_address`
* network: **Новый ресурс:** `uzc_static_nat`
* network: **Новый ресурс:** `uzc_firewall_rule`
* network: **Новый ресурс:** `uzc_port_forwarding_rule`
* network: **Новый ресурс:** `uzc_egress_firewall_rule`
* network: **Новый датасорс:** `uzc_networks`
* network: **Новый датасорс:** `uzc_network`
* network: **Новый датасорс:** `uzc_vpcs`
* network: **Новый датасорс:** `uzc_ip_addresses`
* network: **Новый датасорс:** `uzc_network_categories`
* network: **Новый датасорс:** `uzc_virtual_routers`

* vpn: **Новый ресурс:** `uzc_remote_access_vpn`
* vpn: **Новый ресурс:** `uzc_vpn_gateway`
* vpn: **Новый ресурс:** `uzc_vpn_user`
* vpn: **Новый ресурс:** `uzc_vpn_customer_gateway`

* dns: **Новый ресурс:** `uzc_dns_domain`
* dns: **Новый ресурс:** `uzc_dns_record`
* dns: **Новый датасорс:** `uzc_dns_domains`

* loadbalancer: **Новый ресурс:** `uzc_load_balancer`
* loadbalancer: **Новый ресурс:** `uzc_load_balancer_rule`
* loadbalancer: **Новый датасорс:** `uzc_load_balancers`

* kubernetes: **Новый ресурс:** `uzc_kubernetes_cluster`
* kubernetes: **Новый датасорс:** `uzc_kubernetes_clusters`

* autoscale: **Новый ресурс:** `uzc_vm_autoscale`
* autoscale: **Новый ресурс:** `uzc_vm_autoscale_policy`
* autoscale: **Новый ресурс:** `uzc_vm_autoscale_condition`
* autoscale: **Новый датасорс:** `uzc_autoscales`

* monitoring: **Новый датасорс:** `uzc_monitoring_charts`
* provider: **Новый датасорс:** `uzc_regions`
* provider: **Новый датасорс:** `uzc_region`
* provider: **Новый датасорс:** `uzc_cloud_providers`
* provider: **Новый датасорс:** `uzc_plans`
* provider: **Новый датасорс:** `uzc_storage_categories`
* provider: **Новый датасорс:** `uzc_billing_cycles`
* provider: **Новый датасорс:** `uzc_account_balance`

##### УЛУЧШЕНИЯ:

* provider: `project`, `region`, `cloud_provider` и `billing_cycle` можно задать один раз в блоке провайдера, и они наследуются любым ресурсом, который не задал соответствующий атрибут сам
* provider: асинхронное удаление через биллинговые cancel-requests (`uzc_project`, `uzc_network`, `uzc_vpc` и большинство других биллингуемых сервисов) задокументировано в каждом ресурсе отдельно: `terraform destroy` завершается успешно сразу после принятия запроса на отмену, а не после фактического удаления ресурса
* compute: удаление `uzc_virtual_machine`, в отличие от большинства других ресурсов, дожидается (polling) подтверждения, что VM реально удалена, прежде чем завершиться
* network: ресурсы-правила без эндпоинта удаления (`uzc_static_nat`, `uzc_vpc_acl_rule`, `uzc_load_balancer_rule`, `uzc_dns_record`) при `destroy` падают с явной ошибкой вместо того, чтобы молча разъезжаться со state
* docs: референсная документация по каждому ресурсу и датасорсу генерируется через `tfplugindocs`, включая готовые к запуску сквозные сценарии от `examples/01-minimal-vm` до `examples/06-full-project-stack`
* docs: README доступен на английском и русском (`README.md` / `README.ru.md`)

##### ВНИМАНИЕ:

* kubernetes: API UzCloud пока не предоставляет эндпоинт для получения kubeconfig кластера; `uzc_kubernetes_cluster` может создать кластер, но не может передать его провайдеру `kubernetes`/`helm` в рамках того же запуска
