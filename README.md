# OpenShift NetworkPolicy Konfigürasyonu

Bu repo, OpenShift üzerinde belirli bir namespace'teki `httpd` servisine erişimi kısıtlayan ve yalnızca belirli kurallara izin veren bir NetworkPolicy konfigürasyonu içermektedir.

## Gereksinimler

1. **Hiçbir yere erişim olmasın**: Pod'lar dışarıya erişemez.
2. **Sadece `db` namespace'ine erişim olsun**: Pod'lar yalnızca `db` namespace'inden gelen trafiğe izin verir.
3. **Dış dünyadan sadece `httpd` servisine istek gelsin**: Dışarıdan yalnızca `httpd` servisine (port 80 üzerinden) istek kabul edilir.

## NetworkPolicy YAML

`network-policy.yaml` dosyası aşağıdaki konfigürasyonu içermektedir:

- **Pod Seçimi**: `app: httpd` etiketine sahip pod'ları hedef alır.
- **Gelen Trafik (Ingress)**:
  - `db` namespace'inden gelen tüm trafiğe izin verir.
  - Dış dünyadan (OpenShift router'ı üzerinden) `httpd` servisine gelen trafiğe izin verir (port 80).
- **Giden Trafik (Egress)**:
  - Pod'lar yalnızca `db` namespace'ine trafik gönderebilir.

## NetworkPolicy'yi Uygulama

1. OpenShift kümenizde NetworkPolicy'leri uygulama yetkinizin olduğundan emin olun.
2. Aşağıdaki komutu kullanarak NetworkPolicy'yi uygulayın:

   ```bash
   oc apply -f network-policy.yaml
