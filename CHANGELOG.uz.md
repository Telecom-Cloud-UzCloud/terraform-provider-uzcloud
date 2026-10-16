# Changelog

[🇬🇧 English](CHANGELOG.md) | [🇷🇺 Русский](CHANGELOG.ru.md) | [🇺🇿 O'zbekcha](CHANGELOG.uz.md)

## Unreleased

## 1.1.0 (2026-yil 16-oktabr)

##### YANGI:

* storage: **Yangi resurs:** `uzc_object_storage`, S3 bilan mos obyektli xotira (Ceph): yaratish, import, `size_gb` va `billing_cycle`ni joyida o'zgartirish, `s3_endpoint` eksporti
* kubernetes: `uzc_kubernetes_cluster` UzCloud konsolidagi kabi `master_plan`, `worker_plan`, `blockstorage_plan` va `cloud_provider_setup`ni qabul qiladi; `plan` ishlashda davom etadi va worker tarifi sifatida ishlatiladi
* provider: har qanday asinxron operatsiyani kutishni cheklovchi yangi `operation_timeout_minutes` sozlamasi (yoki `UZC_OPERATION_TIMEOUT`, standart 20)

##### YAXSHILANISHLAR:

* provider: har bir resurs UzCloud operatsiya natijasini tasdiqlashini kutadi (yaratilgan resurs o'qiladi, o'zgarish ko'rinadi, o'chirilgan resurs yo'qoladi) va `operation_timeout_minutes` o'tgach aniq xato bilan to'xtaydi; `uzc_virtual_machine` o'zining yaratish, o'zgartirish va o'chirish taymautlarini saqlaydi
* provider: API xatolari faqat «Validation errors» emas, balki UzCloud yuboradigan maydonlar bo'yicha validatsiya tafsilotlarini ham o'z ichiga oladi, masalan `[storage_category: The storage category field is required.]`
* storage: `uzc_vm_snapshot`, `uzc_blockstorage_snapshot`, `uzc_vm_backup` va `uzc_blockstorage_backup` standart holatda `hourly` billing davridan foydalanadi: foydalanishga qarab hisoblanadigan bu xizmatlar uchun API oylik va yillik davrlarni rad etadi
* storage: `uzc_blockstorage_attachment` disk ulangan (o'chirishda esa uzilgan) deb ko'rsatilguncha kutadi; disk snapshoti disk Ready bo'lguncha qayta yuboriladi; disk uzilishi tugaguncha uni o'chirish qayta urinib ko'riladi
* compute: ochiq IPni kutish 10 daqiqa bilan cheklangan va IP-manzillar kvotasi haqida maslahat bilan tugaydi; kutishi muvaffaqiyatsiz bo'lgan VM boshqaruvsiz ishlab qolish o'rniga state'da tainted sifatida saqlanadi
* compute: VMning zaxira nusxalash jadvali hali bekor qilinayotgan bo'lsa, VMni o'chirish qayta urinib ko'riladi
* network: `uzc_vpc`, `uzc_network`, `uzc_project` va `uzc_vpn_customer_gateway` nomini o'zgartirish resursni qayta o'qish orqali tekshiriladi
* tests: har bir resurs guruhi uchun acceptance testlar, barcha datasource'lar uchun bepul smoke test va akkauntning tariflari, bulutli provayderlari, regionlari hamda xom API javoblarini chiqaradigan razvedka testlari

##### TUZATISHLAR:

* compute: `uzc_virtual_machine`ni yaratish `offering`/`ipaddresses`da «Value Conversion Error» bilan to'xtardi
* compute: VM yaratish so'rovi konsol bilan mos keladi (`username`, bo'sh satrlar o'rniga `null`); nomini o'zgartirish `vm_label`ni yuboradi
* network: `uzc_vpc` prefiksli CIDRni rad etardi («The cidr field must be a valid IP address»); endi `10.0.0.0/16` ham, `10.0.0.0` ham qabul qilinadi
* network: provayder blokidan meros qilib olingan `region`, `project` va `billing_cycle` `uzc_vpc`, `uzc_network`, `uzc_ip_address` va `uzc_virtual_machine` yaratilgandan keyin noma'lum qolardi
* network: o'zgartirishga bo'sh javob `uzc_vpc`, `uzc_network` va `uzc_vpn_customer_gateway` slug'ini o'chirib yuborardi, keyin esa o'chirish buzilardi
* project: `uzc_project` yaratish 422 bilan to'xtardi (endi `icon` har doim yuboriladi, standart `cloud-15`, `status` esa 1/0 ko'rinishida); satr ko'rinishida kelgan status tahlil qilinadi; o'chirish `DELETE /projects/{slug}` orqali amalga oshiriladi
* provider: multipart so'rovlardagi mantiqiy maydonlar Laravel validatsiyasi talab qilganidek 1/0 ko'rinishida yuboriladi (u «true»/«false»ni rad etadi)
* provider: har qanday kutilmagan 4xx javob (400, 405, 409 va h.k.) xato hisoblanadi; ilgari u muvaffaqiyat deb hisoblanardi
* vpn: `uzc_vpn_customer_gateway` `dead_peer_detection`ni yuboradi (spetsifikatsiyadagi yozilish rad etilardi) va o'qishda `gateway`ni saqlaydi; `uzc_vpn_user` yaratilgandan keyin o'z slug'ini topadi
* storage: API `storage_category`ni qaytarmagani uchun `uzc_blockstorage` endi qayta yaratishni talab qilmaydi (u `storage_setting`dan o'qiladi); `uzc_blockstorage_attachment` yangilashda state'dan tushib qolmaydi
* data sources: `uzc_plans` va `uzc_account_balance` satr ko'rinishida yuborilgan sonlarni va bo'sh massiv ko'rinishida yuborilgan atributlarni qabul qiladi

##### DIQQAT:

* provider: `terraform destroy` endi bekor qilish so'rovi qabul qilingach darhol tugamaydi, balki resurslar haqiqatan o'chirilishini kutadi; o'chirish ko'proq vaqt oladi va `operation_timeout_minutes` o'tgach xato bilan to'xtaydi
* storage: snapshot va zaxira nusxalar endi standart holatda `hourly`dan foydalanadi; boshqa davr kerak bo'lsa, `billing_cycle`ni aniq belgilang
* api: provayderda tuzatib bo'lmaydigan UzCloud tomonidagi ma'lum muammolar: VM nomini o'zgartirish xato beradi (`chageLabel`), VPC nomini o'zgartirish qabul qilinadi, lekin qo'llanilmaydi, SSH-kalit qo'shish 500 qaytaradi, loyihani o'chirish rad etiladi («xizmatlari bor loyihalarni o'chirib bo'lmaydi»), test akkauntida Kubernetes yaratish «not configured properly» deb javob beradi

## 1.0.2 (2026-yil 4-sentabr)

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
