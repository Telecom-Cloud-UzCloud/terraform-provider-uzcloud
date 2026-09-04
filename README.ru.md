# Terraform UzCloud Provider

[🇬🇧 English](README.md) | [🇷🇺 Русский](README.ru.md)

Terraform-провайдер для [UzCloud](https://uzcloud.uz/), построенный на [Terraform Plugin Framework](https://developer.hashicorp.com/terraform/plugin/framework).

Полная документация и примеры по каждому ресурсу и датасорсу опубликованы в [Terraform Registry](https://registry.terraform.io/providers/telecom-cloud-uzcloud/uzcloud/latest/docs).

Registry-адрес провайдера: `telecom-cloud-uzcloud/uzcloud`.

## Поддерживаемые ресурсы и датасорсы

### Ресурсы

**Вычисления**

* [uzc_virtual_machine](docs/resources/virtual_machine.md) - виртуальная машина;
* [uzc_ssh_key](docs/resources/ssh_key.md) - публичный SSH-ключ для входа на VM;
* [uzc_template](docs/resources/template.md) - собственный шаблон/образ в аккаунте пользователя;
* [uzc_iso](docs/resources/iso.md) - ISO-образ, загруженный по URL;
* [uzc_vm_snapshot](docs/resources/vm_snapshot.md) - снапшот виртуальной машины;
* [uzc_vm_snapshot_revert](docs/resources/vm_snapshot_revert.md) - откат виртуальной машины к снапшоту;
* [uzc_vm_backup](docs/resources/vm_backup.md) - расписание регулярного резервного копирования VM;
* [uzc_affinity_group](docs/resources/affinity_group.md) - группа (анти-)affinity для размещения VM;

**Хранилище**

* [uzc_blockstorage](docs/resources/blockstorage.md) - блочное хранилище (диск);
* [uzc_blockstorage_attachment](docs/resources/blockstorage_attachment.md) - подключение блочного диска к виртуальной машине;
* [uzc_blockstorage_snapshot](docs/resources/blockstorage_snapshot.md) - снапшот блочного диска;
* [uzc_blockstorage_snapshot_revert](docs/resources/blockstorage_snapshot_revert.md) - откат блочного диска к снапшоту;
* [uzc_blockstorage_backup](docs/resources/blockstorage_backup.md) - расписание регулярного резервного копирования блочного диска;

**Сеть**

* [uzc_network](docs/resources/network.md) - изолированная или L2-сеть;
* [uzc_vpc](docs/resources/vpc.md) - сеть VPC;
* [uzc_vpc_restart](docs/resources/vpc_restart.md) - перезапуск сети VPC;
* [uzc_vpc_acl_rule](docs/resources/vpc_acl_rule.md) - правило сетевого ACL на VPC;
* [uzc_virtual_router](docs/resources/virtual_router.md) - виртуальный маршрутизатор;
* [uzc_ip_address](docs/resources/ip_address.md) - публичный или приватный IP-адрес;
* [uzc_static_nat](docs/resources/static_nat.md) - статический NAT с IP-адреса на виртуальную машину;
* [uzc_firewall_rule](docs/resources/firewall_rule.md) - правило файрвола на IP-адресе;
* [uzc_port_forwarding_rule](docs/resources/port_forwarding_rule.md) - правило проброса портов на IP-адресе;
* [uzc_egress_firewall_rule](docs/resources/egress_firewall_rule.md) - исходящее правило файрвола на изолированной сети;
* [uzc_remote_access_vpn](docs/resources/remote_access_vpn.md) - удалённый доступ по VPN на IP-адресе;
* [uzc_vpn_gateway](docs/resources/vpn_gateway.md) - шлюз site-to-site VPN на VPC;
* [uzc_vpn_user](docs/resources/vpn_user.md) - пользователь VPN удалённого доступа;
* [uzc_vpn_customer_gateway](docs/resources/vpn_customer_gateway.md) - клиентский шлюз site-to-site VPN;
* [uzc_dns_domain](docs/resources/dns_domain.md) - DNS-домен;
* [uzc_dns_record](docs/resources/dns_record.md) - DNS-запись в домене;

**Managed-сервисы**

* [uzc_load_balancer](docs/resources/load_balancer.md) - балансировщик нагрузки;
* [uzc_load_balancer_rule](docs/resources/load_balancer_rule.md) - дополнительное правило на существующем балансировщике;
* [uzc_kubernetes_cluster](docs/resources/kubernetes_cluster.md) - управляемый кластер Kubernetes;
* [uzc_vm_autoscale](docs/resources/vm_autoscale.md) - группа автомасштабирования VM;
* [uzc_vm_autoscale_policy](docs/resources/vm_autoscale_policy.md) - политика увеличения группы автомасштабирования;
* [uzc_vm_autoscale_condition](docs/resources/vm_autoscale_condition.md) - условие уменьшения группы автомасштабирования;

**Организация**

* [uzc_project](docs/resources/project.md) - проект (объединяет ресурсы выше);

### Датасорсы

* [uzc_regions](docs/data-sources/regions.md) / [uzc_region](docs/data-sources/region.md) - доступные регионы;
* [uzc_cloud_providers](docs/data-sources/cloud_providers.md) - доступные облачные провайдеры;
* [uzc_projects](docs/data-sources/projects.md) / [uzc_project](docs/data-sources/project.md) - проекты в аккаунте;
* [uzc_plans](docs/data-sources/plans.md) - доступные тарифные планы;
* [uzc_templates](docs/data-sources/templates.md) - каталог маркетплейс/ОС-шаблонов;
* [uzc_account_templates](docs/data-sources/account_templates.md) - шаблоны в собственном аккаунте;
* [uzc_marketplace_apps](docs/data-sources/marketplace_apps.md) - каталог приложений маркетплейса;
* [uzc_storage_categories](docs/data-sources/storage_categories.md) - доступные категории хранилища;
* [uzc_billing_cycles](docs/data-sources/billing_cycles.md) - доступные биллинговые циклы;
* [uzc_ssh_keys](docs/data-sources/ssh_keys.md) - SSH-ключи в аккаунте;
* [uzc_account_balance](docs/data-sources/account_balance.md) - текущий баланс аккаунта;
* [uzc_networks](docs/data-sources/networks.md) / [uzc_network](docs/data-sources/network.md) - изолированные/L2-сети;
* [uzc_vpcs](docs/data-sources/vpcs.md) - сети VPC;
* [uzc_ip_addresses](docs/data-sources/ip_addresses.md) - выделенные IP-адреса;
* [uzc_network_categories](docs/data-sources/network_categories.md) - доступные категории сетей;
* [uzc_virtual_routers](docs/data-sources/virtual_routers.md) - виртуальные маршрутизаторы;
* [uzc_affinity_groups](docs/data-sources/affinity_groups.md) - группы (анти-)affinity;
* [uzc_blockstorages](docs/data-sources/blockstorages.md) / [uzc_blockstorage](docs/data-sources/blockstorage.md) - блочные диски;
* [uzc_vm_snapshots](docs/data-sources/vm_snapshots.md) - снапшоты виртуальных машин;
* [uzc_blockstorage_snapshots](docs/data-sources/blockstorage_snapshots.md) - снапшоты блочных дисков;
* [uzc_isos](docs/data-sources/isos.md) - ISO-образы в аккаунте;
* [uzc_load_balancers](docs/data-sources/load_balancers.md) - балансировщики нагрузки;
* [uzc_kubernetes_clusters](docs/data-sources/kubernetes_clusters.md) - управляемые кластеры Kubernetes;
* [uzc_autoscales](docs/data-sources/autoscales.md) - группы автомасштабирования VM;
* [uzc_dns_domains](docs/data-sources/dns_domains.md) - DNS-домены;
* [uzc_monitoring_charts](docs/data-sources/monitoring_charts.md) - доступные определения графиков мониторинга;

## Требования

- [Terraform](https://developer.hashicorp.com/terraform/downloads) >= 1.0
- [Go](https://go.dev/doc/install) >= 1.23 (нужен только для сборки провайдера из исходников)
- Аккаунт UzCloud и API-токен (портал UzCloud -> Profile -> API)

## Использование провайдера

```hcl
terraform {
  required_providers {
    uzc = {
      source  = "telecom-cloud-uzcloud/uzcloud"
      version = ">= 1.0.2"
    }
  }
}

provider "uzc" {
  api_token = var.uzc_api_token # или задайте UZC_API_TOKEN
}
```

Полный список опций уровня провайдера, включая наследуемые по умолчанию `project`, `region`,
`cloud_provider` и `billing_cycle`, смотрите в [`examples/provider/provider.tf`](examples/provider/provider.tf).
Готовые к запуску сквозные сценарии (минимальная VM, полный стек проекта, кластер Kubernetes и
другие) лежат в [`examples/`](examples), пронумерованы примерно по возрастанию сложности.

> [!NOTE]
> API-токены UzCloud всегда имеют `expires_at` и рано или поздно перестают работать. Если
> конфигурация, которая раньше применялась без проблем, начала падать с `401 Unauthorized`,
> сначала перевыпустите токен в портале UzCloud, и только потом ищите ошибку в конфигурации
> или в самом провайдере.

### Аутентификация

```bash
export UZC_API_TOKEN=<api-token>     # обязательно; портал UzCloud -> Profile -> API
export UZC_API_ENDPOINT=<base-url>   # опционально; по умолчанию продакшн API
```

### Пример: проект, SSH-ключ и виртуальная машина

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

Более полные примеры по каждому ресурсу (включая альтернативу с `custom_plan` и инструкции
по импорту) лежат в [`examples/resources`](examples/resources).

## Сборка

```bash
make install
# или
go build ./...
```

## Тестирование

### Unit- и клиентские тесты (без кредов, без сети)

Герметичные тесты собственной логики провайдера: валидаторы и хелперы, парсеры import-id, а
также кодирование запросов/ответов HTTP-клиента и маппинг ошибок (через `httptest`).
Конфигурация не требуется:

```bash
make test
# или
go test ./...
```

### Acceptance-тесты

Acceptance-тесты прогоняют полный жизненный цикл ресурса (create, read, update, delete), цикл
импорта и стабильность плана через настоящий Terraform runtime. Как описано в
[руководстве HashiCorp по acceptance-тестам](https://developer.hashicorp.com/terraform/plugin/testing/acceptance-tests),
они создают реальную инфраструктуру и могут стоить денег, поэтому запускаются только при
установленной переменной `TF_ACC`. Без неё, например в CI без секретов, они пропускаются.

Задайте креды провайдера и переменные для тестового окружения, затем запустите:

```bash
export TF_ACC=1
export UZC_API_TOKEN=<api-token>
export UZC_TEST_REGION=<region>
export UZC_TEST_CLOUD_PROVIDER=<cloud-provider>
export UZC_TEST_TEMPLATE=<template>
export UZC_TEST_PLAN=<plan>
export UZC_TEST_SSH_KEY=<ssh-key>

make testacc
```

## Документация

Референсная документация по каждому ресурсу и датасорсу генерируется из описаний схем и
директории `examples/` в [`docs/`](docs) через
[`tfplugindocs`](https://github.com/hashicorp/terraform-plugin-docs):

```bash
make docs
```

Запускайте это после любого изменения `Description` в схеме или любого файла в `examples/`,
и коммитьте получившийся diff `docs/`; CI проверяет, что они синхронизированы.

## Разработка провайдера

```bash
go build ./...
go vet ./...
go test ./...
```

```bash
# Запуск провайдера на локальной конфигурации Terraform без публикации в registry:
go build -o terraform-provider-uzcloud
```

затем укажите в `.terraformrc` блок [`dev_overrides`](https://developer.hashicorp.com/terraform/cli/config/config-file#development-overrides-for-provider-developers)
на собранный бинарник.

## Лицензия

[Mozilla Public License 2.0](LICENSE)
