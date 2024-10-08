
# kubernetesアップデートしたときの記録
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

- OSとcontainrdのバージョンあげてノードを一新してから、k8sバージョンあげる

![画像1](./img/wk3-join.PNG)

- ここまできた

![画像2](./img/replace-worker.PNG)

![画像2](./img/cp-join.PNG)

- メモ：init, join用YAMLは、apiVersionは同じだったためか、特に変更もなかった

- メモ：cp1で`kubeadm config images pull`実行で`pause:3.9`警告でた。一応以下で解消できそう
この修正をするとjoin上手くいった。
```bash
$ vim /etc/containerd/config.toml
sandbox_image = "registry.k8s.io/pause:3.9"
```

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
