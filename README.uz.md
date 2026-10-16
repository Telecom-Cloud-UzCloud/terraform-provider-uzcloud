# Terraform UzCloud Provider

[🇬🇧 English](README.md) | [🇷🇺 Русский](README.ru.md) | [🇺🇿 O'zbekcha](README.uz.md)

[UzCloud](https://uzcloud.uz/) uchun [Terraform Plugin Framework](https://developer.hashicorp.com/terraform/plugin/framework) asosida yaratilgan Terraform provayderi.

Har bir resurs va datasource bo'yicha to'liq hujjatlar va misollar [Terraform Registry](https://registry.terraform.io/providers/telecom-cloud-uzcloud/uzcloud/latest/docs)da e'lon qilingan.

Provayderning Terraform Registry manzili: `telecom-cloud-uzcloud/uzcloud`.

## Qo'llab-quvvatlanadigan resurslar va datasource'lar

### Resurslar

**Hisoblash**

* [uzc_virtual_machine](docs/resources/virtual_machine.md) - virtual mashina;
* [uzc_ssh_key](docs/resources/ssh_key.md) - VMga kirish uchun ochiq SSH-kalit;
* [uzc_template](docs/resources/template.md) - foydalanuvchi akkauntidagi shaxsiy shablon/image;
* [uzc_iso](docs/resources/iso.md) - URL orqali yuklangan ISO-image;
* [uzc_vm_snapshot](docs/resources/vm_snapshot.md) - virtual mashinaning muayyan vaqtdagi snapshoti;
* [uzc_vm_snapshot_revert](docs/resources/vm_snapshot_revert.md) - virtual mashinani snapshotga qaytaradi;
* [uzc_vm_backup](docs/resources/vm_backup.md) - virtual mashina uchun davriy zaxira nusxalash jadvali;
* [uzc_affinity_group](docs/resources/affinity_group.md) - VM joylashuvi uchun (anti-)affinity guruhi;

**Xotira**

* [uzc_blockstorage](docs/resources/blockstorage.md) - blokli xotira (disk);
* [uzc_blockstorage_attachment](docs/resources/blockstorage_attachment.md) - blokli diskni virtual mashinaga ulash;
* [uzc_blockstorage_snapshot](docs/resources/blockstorage_snapshot.md) - blokli diskning snapshoti;
* [uzc_blockstorage_snapshot_revert](docs/resources/blockstorage_snapshot_revert.md) - blokli diskni snapshotga qaytaradi;
* [uzc_blockstorage_backup](docs/resources/blockstorage_backup.md) - blokli disk uchun davriy zaxira nusxalash jadvali;
* [uzc_object_storage](docs/resources/object_storage.md) - S3 bilan mos obyektli xotira (Ceph);

**Tarmoq**

* [uzc_network](docs/resources/network.md) - izolyatsiyalangan yoki L2-tarmoq;
* [uzc_vpc](docs/resources/vpc.md) - VPC tarmog'i;
* [uzc_vpc_restart](docs/resources/vpc_restart.md) - VPC tarmog'ini qayta ishga tushirish;
* [uzc_vpc_acl_rule](docs/resources/vpc_acl_rule.md) - VPC uchun tarmoq ACL qoidasi;
* [uzc_virtual_router](docs/resources/virtual_router.md) - virtual router;
* [uzc_ip_address](docs/resources/ip_address.md) - ochiq yoki xususiy IP-manzil;
* [uzc_static_nat](docs/resources/static_nat.md) - IP-manzildan virtual mashinaga statik NAT;
* [uzc_firewall_rule](docs/resources/firewall_rule.md) - IP-manzildagi firewall qoidasi;
* [uzc_port_forwarding_rule](docs/resources/port_forwarding_rule.md) - IP-manzildagi port forwarding qoidasi;
* [uzc_egress_firewall_rule](docs/resources/egress_firewall_rule.md) - izolyatsiyalangan tarmoqdagi chiquvchi firewall qoidasi;
* [uzc_remote_access_vpn](docs/resources/remote_access_vpn.md) - IP-manzildagi masofaviy kirish VPN;
* [uzc_vpn_gateway](docs/resources/vpn_gateway.md) - VPC uchun site-to-site VPN shlyuzi;
* [uzc_vpn_user](docs/resources/vpn_user.md) - masofaviy kirish VPN foydalanuvchisi;
* [uzc_vpn_customer_gateway](docs/resources/vpn_customer_gateway.md) - site-to-site VPN mijoz shlyuzi;
* [uzc_dns_domain](docs/resources/dns_domain.md) - DNS-domen;
* [uzc_dns_record](docs/resources/dns_record.md) - domendagi DNS-yozuv;

**Boshqariladigan xizmatlar**

* [uzc_load_balancer](docs/resources/load_balancer.md) - yuklamani muvozanatlash (load balancer);
* [uzc_load_balancer_rule](docs/resources/load_balancer_rule.md) - mavjud load balancer uchun qo'shimcha qoida;
* [uzc_kubernetes_cluster](docs/resources/kubernetes_cluster.md) - boshqariladigan Kubernetes klasteri;
* [uzc_vm_autoscale](docs/resources/vm_autoscale.md) - VM avtomasshtablash guruhi;
* [uzc_vm_autoscale_policy](docs/resources/vm_autoscale_policy.md) - avtomasshtablash guruhini kengaytirish siyosati;
* [uzc_vm_autoscale_condition](docs/resources/vm_autoscale_condition.md) - avtomasshtablash guruhini qisqartirish sharti;

**Tashkilot**

* [uzc_project](docs/resources/project.md) - loyiha (yuqoridagi resurslarni birlashtiradi);

### Datasource'lar

* [uzc_regions](docs/data-sources/regions.md) / [uzc_region](docs/data-sources/region.md) - mavjud regionlar;
* [uzc_cloud_providers](docs/data-sources/cloud_providers.md) - mavjud bulutli provayderlar;
* [uzc_projects](docs/data-sources/projects.md) / [uzc_project](docs/data-sources/project.md) - akkauntdagi loyihalar;
* [uzc_plans](docs/data-sources/plans.md) - mavjud tarif rejalari;
* [uzc_templates](docs/data-sources/templates.md) - marketplace/OS shablonlari katalogi;
* [uzc_account_templates](docs/data-sources/account_templates.md) - foydalanuvchi akkauntidagi shablonlar;
* [uzc_marketplace_apps](docs/data-sources/marketplace_apps.md) - marketplace ilovalari katalogi;
* [uzc_storage_categories](docs/data-sources/storage_categories.md) - mavjud xotira kategoriyalari;
* [uzc_billing_cycles](docs/data-sources/billing_cycles.md) - mavjud billing davrlari;
* [uzc_ssh_keys](docs/data-sources/ssh_keys.md) - akkauntdagi SSH-kalitlar;
* [uzc_account_balance](docs/data-sources/account_balance.md) - joriy akkaunt balansi;
* [uzc_networks](docs/data-sources/networks.md) / [uzc_network](docs/data-sources/network.md) - izolyatsiyalangan/L2-tarmoqlar;
* [uzc_vpcs](docs/data-sources/vpcs.md) - VPC tarmoqlari;
* [uzc_ip_addresses](docs/data-sources/ip_addresses.md) - ajratilgan IP-manzillar;
* [uzc_network_categories](docs/data-sources/network_categories.md) - mavjud tarmoq kategoriyalari;
* [uzc_virtual_routers](docs/data-sources/virtual_routers.md) - virtual routerlar;
* [uzc_affinity_groups](docs/data-sources/affinity_groups.md) - (anti-)affinity guruhlari;
* [uzc_blockstorages](docs/data-sources/blockstorages.md) / [uzc_blockstorage](docs/data-sources/blockstorage.md) - blokli disklar;
* [uzc_vm_snapshots](docs/data-sources/vm_snapshots.md) - virtual mashina snapshotlari;
* [uzc_blockstorage_snapshots](docs/data-sources/blockstorage_snapshots.md) - blokli disk snapshotlari;
* [uzc_isos](docs/data-sources/isos.md) - akkauntdagi ISO-image'lar;
* [uzc_load_balancers](docs/data-sources/load_balancers.md) - load balancerlar;
* [uzc_kubernetes_clusters](docs/data-sources/kubernetes_clusters.md) - boshqariladigan Kubernetes klasterlari;
* [uzc_autoscales](docs/data-sources/autoscales.md) - VM avtomasshtablash guruhlari;
* [uzc_dns_domains](docs/data-sources/dns_domains.md) - DNS-domenlar;
* [uzc_monitoring_charts](docs/data-sources/monitoring_charts.md) - mavjud monitoring grafik ta'riflari;

## Talablar

- [Terraform](https://developer.hashicorp.com/terraform/downloads) >= 1.0
- [Go](https://go.dev/doc/install) >= 1.23 (faqat provayderni manba kodidan yig'ish uchun kerak)
- UzCloud akkaunti va API-token (UzCloud portali -> Profile -> API)

## Provayderdan foydalanish

```hcl
terraform {
  required_providers {
    uzc = {
      source  = "telecom-cloud-uzcloud/uzcloud"
      version = ">= 1.1.0"
    }
  }
}

provider "uzc" {
  api_token = var.uzc_api_token # yoki UZC_API_TOKEN o'rnating
}
```

Provayder darajasidagi barcha sozlamalar, jumladan `project`, `region`, `cloud_provider`
va `billing_cycle` uchun meros qilib olinadigan standart qiymatlar
[`examples/provider/provider.tf`](examples/provider/provider.tf) faylida keltirilgan.
Ishga tushirishga tayyor end-to-end stsenariylar (minimal VM, to'liq loyiha steki, Kubernetes
klasteri va boshqalar) [`examples/`](examples) papkasida, taxminan murakkablik darajasi
bo'yicha raqamlangan holda joylashgan.

> [!NOTE]
> UzCloud API-tokenlari har doim `expires_at` ga ega bo'lib, oxir-oqibat ishlamay qoladi.
> Agar avval muammosiz qo'llanilgan konfiguratsiya `401 Unauthorized` xatosi bilan
> ishlamay qolsa, avval UzCloud portalidan tokenni qayta chiqaring, so'ng konfiguratsiya
> yoki provayderning o'zidagi xatoni qidiring.

### Autentifikatsiya

```bash
export UZC_API_TOKEN=<api-token>     # majburiy; UzCloud portali -> Profile -> API
export UZC_API_ENDPOINT=<base-url>   # ixtiyoriy; standart holatda production API
```

### Misol: loyiha, SSH-kalit va virtual mashina

```hcl
resource "uzc_project" "example" {
  name        = "example-project"
  description = "Managed by Terraform"
}

resource "uzc_ssh_key" "example" {
  name       = "example-key"
  public_key = file("~/.ssh/id_ed25519.pub")
  project    = uzc_project.example.slug
}

resource "uzc_virtual_machine" "example" {
  name           = "example-vm"
  cloud_provider = "nimbo"
  project        = uzc_project.example.slug
  region         = "tashkent"
  billing_cycle  = "hourly"

  boot_source = "image"
  template    = "ubuntu-22-04"
  plan        = "box2cm2"

  is_public = true
  ssh_key   = uzc_ssh_key.example.slug
}
```

Har bir resurs bo'yicha to'liqroq misollar (jumladan `custom_plan` o'lchami muqobili va
import bo'yicha ko'rsatmalar) [`examples/resources`](examples/resources) papkasida joylashgan.

## Yig'ish

```bash
make install
# yoki
go build ./...
```

## Testlash

### Unit va klient testlari (kredensiallarsiz, tarmoqsiz)

Provayderning o'z mantig'ini tekshiruvchi germetik testlar: validatorlar va yordamchi
funksiyalar, import-id parserlari, shuningdek HTTP-klientning so'rov/javob kodlashtirishi
va xatolarni moslashtirishi (`httptest` orqali). Hech qanday konfiguratsiya talab qilinmaydi:

```bash
make test
# yoki
go test ./...
```

### Acceptance testlar

Acceptance testlar resursning to'liq hayot siklini (create, read, update, delete), import
tsiklini va reja barqarorligini haqiqiy Terraform runtime orqali sinab ko'radi. HashiCorp'ning
[acceptance testlash bo'yicha qo'llanmasi](https://developer.hashicorp.com/terraform/plugin/testing/acceptance-tests)da
tasvirlanganidek, ular haqiqiy infratuzilma yaratadi va pul talab qilishi mumkin, shuning
uchun faqat `TF_ACC` o'rnatilgan bo'lsa ishga tushadi. U bo'lmasa, masalan sirlarsiz CIda,
ular o'tkazib yuboriladi.

Provayder kredensiallari va test ko'lami o'zgaruvchilarini o'rnating, so'ng ishga tushiring:

```bash
export TF_ACC=1
export UZC_API_TOKEN=<api-token>
export UZC_TEST_PROJECT=<existing-project-slug>
export UZC_TEST_NETWORK=<existing-isolated-network-slug>   # for tests that create a VM
export UZC_TEST_SSH_KEY=<existing-ssh-key-slug>            # for tests that create a VM

make testacc
```

Standart qiymatlar Toshkent regioniga mos va akkauntga qarab o'zgartiriladi:
`UZC_TEST_REGION` (`tashkent`), `UZC_TEST_CLOUD_PROVIDER` (`nimbo`),
`UZC_TEST_BILLING_CYCLE` (`monthly`), `UZC_TEST_TEMPLATE` (`ubuntu-2404`),
`UZC_TEST_PLAN` (`optimal-line-2`), `UZC_TEST_STORAGE_CATEGORY` (`ssd-storage`),
`UZC_TEST_ROOT_DISK_PLAN` (`standard-4`), `UZC_TEST_VOLUME_PLAN` (ildiz disk tarifi),
`UZC_TEST_VPC_PLAN` (`vpc-1`). Akkauntingiz uchun to'g'ri slug'lar `uzc_templates`,
`uzc_plans`, `uzc_storage_categories` va `uzc_billing_cycles` datasource'laridan olinadi.

Hammasini emas, alohida guruhni ishga tushirish mumkin, masalan:

```bash
TF_ACC=1 go test ./internal/provider -run 'TestAccDataSources' -v     # bepul, hech narsa yaratmaydi
TF_ACC=1 go test ./internal/provider -run 'TestAccVirtualMachine_lifecycle' -v -timeout 60m
TF_ACC=1 go test ./internal/provider -run 'TestAccBlockStorage' -v -timeout 90m
```

| Test | Nimani tekshiradi | Nima kerak |
| --- | --- | --- |
| `TestAccDataSources_*` | argumentsiz barcha datasource'lar, tariflar, slug bo'yicha qidiruv | loyiha |
| `TestAccSSHKey_basic`, `TestAccProject_basic` | yaratish, import, nomini o'zgartirish | loyiha |
| `TestAccNetwork_*`, `TestAccVPC_*` | izolyatsiyalangan tarmoq, yangi tarmoqdagi VM, VPC | loyiha; ixtiyoriy `UZC_TEST_NETWORK_PLAN`, `UZC_TEST_VPC_PLAN` |
| `TestAccVirtualMachine_lifecycle` | yaratish, import, joyida nomini o'zgartirish, to'xtatish va ishga tushirish | tarmoq, SSH-kalit |
| `TestAccBlockStorage_*` | ulash, uzish, qayta ulash, bir nechta qo'shimcha disk, yaratishda ulash | tarmoq, SSH-kalit |
| `TestAccObjectStorage_basic` | obyektli xotirani yaratish, import, o'lchamini joyida o'zgartirish | loyiha |
| `TestAccVirtualMachine_public`, `TestAccIPAddress_firewall` | ochiq IP, firewall qoidasi | `UZC_TEST_PUBLIC=1`, `UZC_TEST_IP_PLAN` |
| `TestAccKubernetesCluster_basic` | klaster yaratish, import, ixtiyoriy tarif almashtirish | `UZC_TEST_K8S_VERSION`, `UZC_TEST_K8S_PLAN` |
| `TestAccVMSnapshot_basic` | VM snapshoti | `UZC_TEST_SNAPSHOT_PLAN` |

Bu testlar pullik resurslar yaratadi. `uzc_static_nat` qamrab olinmagan: UzCloudda uni
o'chirish uchun endpoint yo'q, shuning uchun test o'zidan keyin tozalay olmaydi.

## Hujjatlar

Har bir resurs va datasource uchun havola hujjatlar sxema tavsiflari va `examples/`
papkasidan [`tfplugindocs`](https://github.com/hashicorp/terraform-plugin-docs) orqali
[`docs/`](docs) ichiga generatsiya qilinadi:

```bash
make docs
```

Buni sxemadagi har qanday `Description`ni yoki `examples/` ichidagi istalgan faylni
o'zgartirgandan so'ng ishga tushiring va natijaviy `docs/` diffini committing qiling;
CI ularning sinxronligini tekshiradi.

## Provayderni ishlab chiqish

```bash
go build ./...
go vet ./...
go test ./...
```

```bash
# Provayderni registryga chop etmasdan, lokal Terraform konfiguratsiyasiga qarshi ishga
# tushirish:
go build -o terraform-provider-uzcloud
```

so'ngra `.terraformrc` faylida [`dev_overrides`](https://developer.hashicorp.com/terraform/cli/config/config-file#development-overrides-for-provider-developers)
blokini yig'ilgan binary faylga ko'rsating.

## Litsenziya

[Mozilla Public License 2.0](LICENSE)
