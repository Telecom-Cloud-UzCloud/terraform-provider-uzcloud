# Changelog

[🇬🇧 English](CHANGELOG.md) | [🇷🇺 Русский](CHANGELOG.ru.md) | [🇺🇿 O'zbekcha](CHANGELOG.uz.md)

## Unreleased

##### YANGI:

* provider: Terraform Plugin Framework asosida qurilgan UzCloud Terraform provayderining birinchi relizi
* project: **Yangi resurs:** `uzc_project`
* project: **Yangi datasource:** `uzc_projects`
* project: **Yangi datasource:** `uzc_project`

* compute: **Yangi resurs:** `uzc_virtual_machine`
* compute: **Yangi resurs:** `uzc_ssh_key`
* compute: **Yangi resurs:** `uzc_template`
* compute: **Yangi resurs:** `uzc_iso`
* compute: **Yangi resurs:** `uzc_vm_snapshot`
* compute: **Yangi resurs:** `uzc_vm_snapshot_revert`
* compute: **Yangi resurs:** `uzc_vm_backup`
* compute: **Yangi resurs:** `uzc_affinity_group`
* compute: **Yangi datasource:** `uzc_ssh_keys`
* compute: **Yangi datasource:** `uzc_templates`
* compute: **Yangi datasource:** `uzc_account_templates`
* compute: **Yangi datasource:** `uzc_marketplace_apps`
* compute: **Yangi datasource:** `uzc_isos`
* compute: **Yangi datasource:** `uzc_vm_snapshots`
* compute: **Yangi datasource:** `uzc_affinity_groups`

* storage: **Yangi resurs:** `uzc_blockstorage`
* storage: **Yangi resurs:** `uzc_blockstorage_attachment`
* storage: **Yangi resurs:** `uzc_blockstorage_snapshot`
* storage: **Yangi resurs:** `uzc_blockstorage_snapshot_revert`
* storage: **Yangi resurs:** `uzc_blockstorage_backup`
* storage: **Yangi datasource:** `uzc_blockstorages`
* storage: **Yangi datasource:** `uzc_blockstorage`
* storage: **Yangi datasource:** `uzc_blockstorage_snapshots`

* network: **Yangi resurs:** `uzc_network`
* network: **Yangi resurs:** `uzc_vpc`
* network: **Yangi resurs:** `uzc_vpc_restart`
* network: **Yangi resurs:** `uzc_vpc_acl_rule`
* network: **Yangi resurs:** `uzc_virtual_router`
* network: **Yangi resurs:** `uzc_ip_address`
* network: **Yangi resurs:** `uzc_static_nat`
* network: **Yangi resurs:** `uzc_firewall_rule`
* network: **Yangi resurs:** `uzc_port_forwarding_rule`
* network: **Yangi resurs:** `uzc_egress_firewall_rule`
* network: **Yangi datasource:** `uzc_networks`
* network: **Yangi datasource:** `uzc_network`
* network: **Yangi datasource:** `uzc_vpcs`
* network: **Yangi datasource:** `uzc_ip_addresses`
* network: **Yangi datasource:** `uzc_network_categories`
* network: **Yangi datasource:** `uzc_virtual_routers`

* vpn: **Yangi resurs:** `uzc_remote_access_vpn`
* vpn: **Yangi resurs:** `uzc_vpn_gateway`
* vpn: **Yangi resurs:** `uzc_vpn_user`
* vpn: **Yangi resurs:** `uzc_vpn_customer_gateway`

* dns: **Yangi resurs:** `uzc_dns_domain`
* dns: **Yangi resurs:** `uzc_dns_record`
* dns: **Yangi datasource:** `uzc_dns_domains`

* loadbalancer: **Yangi resurs:** `uzc_load_balancer`
* loadbalancer: **Yangi resurs:** `uzc_load_balancer_rule`
* loadbalancer: **Yangi datasource:** `uzc_load_balancers`

* kubernetes: **Yangi resurs:** `uzc_kubernetes_cluster`
* kubernetes: **Yangi datasource:** `uzc_kubernetes_clusters`

* autoscale: **Yangi resurs:** `uzc_vm_autoscale`
* autoscale: **Yangi resurs:** `uzc_vm_autoscale_policy`
* autoscale: **Yangi resurs:** `uzc_vm_autoscale_condition`
* autoscale: **Yangi datasource:** `uzc_autoscales`

* monitoring: **Yangi datasource:** `uzc_monitoring_charts`
* provider: **Yangi datasource:** `uzc_regions`
* provider: **Yangi datasource:** `uzc_region`
* provider: **Yangi datasource:** `uzc_cloud_providers`
* provider: **Yangi datasource:** `uzc_plans`
* provider: **Yangi datasource:** `uzc_storage_categories`
* provider: **Yangi datasource:** `uzc_billing_cycles`
* provider: **Yangi datasource:** `uzc_account_balance`

##### YAXSHILANISHLAR:

* provider: `project`, `region`, `cloud_provider` va `billing_cycle` provayder blokida bir marta o'rnatilishi va mos atributni o'zida belgilamagan har qanday resurs tomonidan meros qilib olinishi mumkin
* provider: billing cancel-request orqali asinxron o'chirish (`uzc_project`, `uzc_network`, `uzc_vpc` va boshqa ko'pchilik billing qilinadigan xizmatlar) har bir resurs uchun alohida hujjatlashtirilgan: `terraform destroy` resurs haqiqatan o'chirilganidan emas, cancel-request qabul qilingandan so'ng darhol muvaffaqiyatli yakunlanadi
* compute: `uzc_virtual_machine`ni o'chirish, ko'pchilik boshqa resurslardan farqli o'laroq, VM haqiqatan yo'q qilinganini tasdiqlagunga qadar kutadi (polling)
* network: o'chirish endpointi bo'lmagan qoida-resurslar (`uzc_static_nat`, `uzc_vpc_acl_rule`, `uzc_load_balancer_rule`, `uzc_dns_record`) `destroy`da state bilan sekin-asta muvofiqsizlashish o'rniga aniq xato bilan to'xtaydi
* docs: har bir resurs va datasource uchun havola hujjatlar `tfplugindocs` orqali generatsiya qilinadi, jumladan `examples/01-minimal-vm`dan `examples/06-full-project-stack`gacha bo'lgan ishga tushirishga tayyor end-to-end stsenariylar
* docs: README ingliz, rus va o'zbek tillarida mavjud (`README.md` / `README.ru.md` / `README.uz.md`)

##### DIQQAT:

* kubernetes: UzCloud API hozircha klaster kubeconfig'ini olish uchun endpoint taqdim etmaydi; `uzc_kubernetes_cluster` klaster yarata oladi, lekin uni bir xil ishga tushirishda `kubernetes`/`helm` provayderiga topshira olmaydi
