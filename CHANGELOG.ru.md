# Changelog

[🇬🇧 English](CHANGELOG.md) | [🇷🇺 Русский](CHANGELOG.ru.md) | [🇺🇿 O'zbekcha](CHANGELOG.uz.md)

## Unreleased

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
