# my-app-k8s-2
> 自宅サーバーkubernetesの第2版

<br><br>

# kubernetesアップデート
> 手順のメモ

## 準備/構成

- 事前タスク
  - **準備としてv1.28のworkerを１台用意**

- 現行k8sクラスタの構成
  - OS：すべて Ubuntu22.04
  - Version：すべて v1.28
  - ControlPlane：k8s-cp-1
  - Worker：k8s-wk-1
  - Worker：k8s-wk-2

## 全体の流れ

### v1.28 → v1.29
- ./v1-28-to-v1-29.md
- ./v1-29-to-v1-30.md

### 最後にcp, wk のノードをクリーンアップする
- 追加したリフレッシュなノードに随時置き換えていく作業を予定
- cpはメインノードの切り替えなどが必要になる？かも


<br><br>

### 参考URL/公式アップデートガイド
```bash
v1.28 → v1.29
https://v1-29.docs.kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/

v1.29 → v1.30
https://v1-30.docs.kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/

ref: drain
https://v1-28.docs.kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
https://v1-29.docs.kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/


### アプリもetcdも今回はバックアップなしで進めてみる
ref: backup
# etcd
https://kubernetes.io/ja/docs/tasks/administer-cluster/configure-upgrade-etcd/#etcd%E3%82%AF%E3%83%A9%E3%82%B9%E3%82%BF%E3%83%BC%E3%81%AE%E3%83%90%E3%83%83%E3%82%AF%E3%82%A2%E3%83%83%E3%83%97

```
