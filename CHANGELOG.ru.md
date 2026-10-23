# Changelog

[🇬🇧 English](CHANGELOG.md) | [🇷🇺 Русский](CHANGELOG.ru.md) | [🇺🇿 O'zbekcha](CHANGELOG.uz.md)

## 1.2.0 (23 октября 2026)

##### НОВОЕ:

* network: `uzc_network` создаёт подсеть (tier) VPC: `type = "Vpc"` с `vpc`, `gateway` и `netmask`, а `acl_id` применяет один из списков сетевого ACL этого VPC (меняется без пересоздания)
* network: **Новый ресурс:** `uzc_network_acl_list`, список сетевого ACL в VPC
* network: **Новый ресурс:** `uzc_network_acl_rule`, правило в списке сетевого ACL, изменяется без пересоздания
* network: **Новый ресурс:** `uzc_vpc_static_route`, статический маршрут VPC
* kubernetes: `uzc_kubernetes_cluster` отдаёт `kubeconfig` кластера (секретный атрибут) и адрес API-сервера `endpoint`, поэтому провайдеры `kubernetes` и `helm` могут работать с кластером, созданным в том же запуске

##### УЛУЧШЕНИЯ:

* compute: перед созданием `uzc_virtual_machine` провайдер сверяет `template`, `plan`, `blockstorage_plan`, `storage_category`, `networks` и `ssh_key` с каталогами аккаунта и сообщает о неизвестном slug с ближайшим совпадением и списком допустимых значений (`ubuntu-24-04` подсказывает `ubuntu-2404`), а не передаёт его в API, который отвечает внутренними ошибками вроде «Call to a member function getTable() on null»
* compute: `server`, `template_type` и `os_family`, если не заданы, по умолчанию равны `cloud-compute`, типу образа шаблона и семейству ОС шаблона, как их заполняет консоль
* compute: новый атрибут `network_plan` для сети, которую UzCloud создаёт, когда `network_type` равен `Isolated`, а `networks` пуст; если он не задан, берётся основной тариф сети аккаунта (или `isolated`)
* network: у `uzc_network` появился атрибут `billing_cycle`; если он не задан, берётся `billing_cycle` провайдера, а затем `hourly`
* compute: если `network_type` не задан, `uzc_virtual_machine` берёт тип сети из `networks`, как это делает консоль; документированное значение теперь `"Vpc"` (`"VPC"` по-прежнему принимается), а подсеть VPC с `network_type = "Isolated"` или обычная сеть с `"Vpc"` отклоняются до запроса на создание
* compute: у `uzc_ssh_key` появился атрибут `region` (если не задан, берётся `region` провайдера), а `project` и `region` считываются при обновлении состояния и импорте
* network: `uzc_vpc` ждёт, пока VPC перейдёт в состояние `Enabled`, чтобы сразу после него можно было добавлять подсети, и отправляет `vr_plan`, как консоль
* kubernetes: изменение `node_size` масштабирует `uzc_kubernetes_cluster` без пересоздания, а запрос на смену тарифа отправляется, только когда изменились тариф или цикл оплаты
* storage: новый атрибут `size_gb` у `uzc_blockstorage`; большее значение увеличивает диск без пересоздания
* loadbalancer: виртуальные машины `uzc_load_balancer_rule` теперь можно не только добавлять, но и убирать без пересоздания
* dns: `uzc_dns_record` меняет `ttl` и `content` без пересоздания, считывает их при обновлении состояния и импортируется как `{domain}/{type}/{name}`
* compute: имя `uzc_ssh_key` длиннее 20 символов, которое UzCloud отклоняет, отклоняется ещё до запроса на создание
* network: переименование `uzc_vpc` ждёт новое имя до двух минут: UzCloud применяет его через несколько секунд после того, как принял запрос
* compute: удаление сети из `networks` у `uzc_virtual_machine` отключает её без пересоздания VM (`POST .../remove-network`)

##### ИСПРАВЛЕНИЯ:

* provider: `cloud_provider` наследуется из блока провайдера, как и описано в документации; раньше он был обязательным во всех вычислительных ресурсах
* provider: проверки «ровно одно из» (`network`/`vpc` у `uzc_ip_address`, `plan`/`custom_plan`, источники `uzc_template`, тарифы Kubernetes) больше не отвергают значение, известное только после apply, например `network = uzc_network.this.slug`
* docs: в примерах были несуществующие slug шаблона и тарифа (`ubuntu-22-04`, `box2cm2`) и не было `storage_category` и `blockstorage_plan`; все шесть сценариев теперь проходят `terraform validate`
* compute: создание VM с `network_type = "Isolated"` без `networks` падало с 422 «The network plan field is required»
* network: `uzc_network` не отправлял `billing_cycle`: тогда UzCloud отвечал 403 «Что-то пошло не так» и оставлял сеть без идентификатора CloudStack, а её переименование падало с «missing parameter id»; теперь `billing_cycle` отправляется всегда
* network: `uzc_network` падал с «The network plan field is required», если `network_plan` не задан; теперь используется основной тариф сети аккаунта (или `isolated`)
* provider: при создании `uzc_network`, `uzc_vpc` и `uzc_load_balancer` отправляется `is_vm_snapshot`, как указано в спецификации API
* data sources: `uzc_load_balancers` падал, когда API присылал порты или флаги правила строкой
* compute: создание `uzc_ssh_key` падало с 500 `Attempt to read property "id" on null`: API нужен регион ключа, теперь он отправляется вместе с проектом
* network: `uzc_egress_firewall_rule` ждал до истечения тайм-аута операции: API не возвращает правило в ответ на создание и называет поля `cidr` и `destcidr_list`; теперь новое правило находится в списке, а порты, которые приходят числом или null, читаются правильно
* network: `uzc_vpc_acl_rule` при создании ждал до истечения тайм-аута операции (API не возвращает id) и всегда падал при удалении; теперь он находит созданный список и удаляет его
* provider: ответ CloudStack 403 «entity does not exist» для удалённого объекта считается признаком «не найдено», поэтому удаление и обновление состояния на нём не падают
* loadbalancer: удаление `uzc_load_balancer_rule` всегда падало; теперь правило удаляется
* dns: удаление `uzc_dns_record` всегда падало; теперь запись удаляется: провайдер отправляет её в том виде, в каком её возвращает домен, как это делает консоль
* network: создание `uzc_network` падало с «cannot unmarshal number into ... is_default of type bool»: ответ на создание присылает флаги сети числами
* network: переименование `uzc_network` падало с «cannot unmarshal array»: в ответе нет сети, теперь она считывается заново, когда новое имя применится
* loadbalancer: добавление `uzc_load_balancer_rule` к существующему балансировщику отвечало 403 «Что-то пошло не так»: теперь, как в консоли, отправляются настройки проверки доступности и `ipaddress` машин
* compute: VM с несколькими сетями в `networks` выдавала «Provider produced inconsistent result», потому что UzCloud возвращает их в другом порядке; теперь сохраняется порядок из конфигурации
* network: удаление `uzc_network` сразу после её последней VM повторяется, пока UzCloud отвечает «You cannot delete the network while virtual machines are created using it»
* network: `uzc_vpc_static_route`, запрошенный сразу после создания подсети, UzCloud принимает, но не создаёт; если маршрут не появился за минуту, запрос отправляется повторно

##### УСТАРЕЛО:

* network: `uzc_vpc_acl_rule` устарел: несмотря на название, он управляет списком сетевого ACL, а не правилом. Используйте `uzc_network_acl_list` (`rule_id` становится `acl_id`) и `uzc_network_acl_rule`

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
