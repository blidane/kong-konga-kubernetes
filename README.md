# kong-konga-external-postgresql-kubernetes
Manual ini memuat instalasi KONG PI gateway dan KONGA sebagai web management, sedangkan database di ASUMSIKAN telah memiliki server database PostgreSQL terpisah yang sudah SIAP, sehingga tinggal membuat database berserta aksesnya, seperti berikut:


```
IP server PostgreSQL: 12.34.56.78
Port		: 5432
Username	: wkongdbuserk8s
Password	: R4ha51iaD0n9!KONG
Database	: wkongdk8s

create database wkongdk8s;
create user wkongdbuserk8s with encrypted password 'R4ha51iaD0n9!KONG';
grant all privileges on database wkongdk8s to wkongdbuserk8s;


IP server PostgreSQL: 12.34.56.78
Port		: 5432
Username	: wkongadbuserk8s
Password	: R4ha51iaD0n9!KONGA
Database	: wkongadbk8s

create database wkongadbk8s;
create user wkongadbuserk8s with encrypted password 'R4ha51iaD0n9!KONGA';
grant all privileges on database wkongadbk8s to wkongadbuserk8s;
```

## Install kong & konga
Asumsi server kubernetes sudah siap pakai, pada tutorial ini menggunakan k3s dan menggunakan script installer k3sup (baca: kecap) agar instalasi kubernetes hanya beberapa detiks

```
1. Install Helm versi 3
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3

2. Ubah mode ke 700
chmod 700 get_helm.sh

3. Jalankan script untuk instalasi Helm
./get_helm.sh

4. Buat Name Scape kong
kubectl create namespace kong

5. Pasang yaml 1-konga-prepare-db-job.yaml
kubectl apply –f 1-konga-prepare-db-job.yaml

6. Tambahkan repo untuk Kong
helm repo add kong https://charts.konghq.com

7. Update Repo
helm repo update

8. Buat pengamanan untuk koneksi kong
kubectl -n kong create secret generic postgres --from-literal=wkongdbuserk8s=R4ha51iaD0n9!KONG

9. Export config k8s, sesuaikan jika menggunakan flatform kubernetes yang lain
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml

10. Update konfigurasi kong dengan versi sesuai kebutuhan
helm upgrade -i blidane kong/kong -f 2-values.yaml --namespace kong --version 2.6.3

11. Install konga sebagai web management 3-konga-deploy.yaml
kubectl apply -f 3-konga-deploy.yaml

12. Expose aplikasi konga agar bisa diakses dari web
kubectl -n kong expose deploy blidane-konga --type=NodePort

13. Cek service-service pada namespace kong
kubectl -n kong get svc

14. Jika ingin statik port, jalankan
kubectl apply -f 4-konga-update-svc.yaml

15. Buka konga di browser, misalnya http://192.168.x.z:30123, lalu isikan sbb:
Name		: kong-root-api
Kong Admin URL	: http://blidane-kong-admin.kong.svc.cluster.local:8001
```
