# crd-test
// TODO(user): Add simple overview of use/purpose

## Description
// TODO(user): An in-depth paragraph about your project and overview of use

## Getting Started

### Prerequisites
- go version v1.21.0+
- docker version 17.03+.
- kubectl version v1.11.3+.
- Access to a Kubernetes v1.11.3+ cluster.

### To Deploy on the cluster
**Build and push your image to the location specified by `IMG`:**

```sh
make docker-build docker-push IMG=<some-registry>/crd-test:tag
```

**NOTE:** This image ought to be published in the personal registry you specified.
And it is required to have access to pull the image from the working environment.
Make sure you have the proper permission to the registry if the above commands don’t work.

**Install the CRDs into the cluster:**

```sh
make install
```

**Deploy the Manager to the cluster with the image specified by `IMG`:**

```sh
make deploy IMG=<some-registry>/crd-test:tag
```

> **NOTE**: If you encounter RBAC errors, you may need to grant yourself cluster-admin
privileges or be logged in as admin.

**Create instances of your solution**
You can apply the samples (examples) from the config/sample:

```sh
kubectl apply -k config/samples/
```

>**NOTE**: Ensure that the samples has default values to test it out.

### To Uninstall
**Delete the instances (CRs) from the cluster:**

```sh
kubectl delete -k config/samples/
```

**Delete the APIs(CRDs) from the cluster:**

```sh
make uninstall
```

**UnDeploy the controller from the cluster:**

```sh
make undeploy
```

## Project Distribution

Following are the steps to build the installer and distribute this project to users.

1. Build the installer for the image built and published in the registry:

```sh
make build-installer IMG=<some-registry>/crd-test:tag
```

NOTE: The makefile target mentioned above generates an 'install.yaml'
file in the dist directory. This file contains all the resources built
with Kustomize, which are necessary to install this project without
its dependencies.

2. Using the installer

Users can just run kubectl apply -f <URL for YAML BUNDLE> to install the project, i.e.:

```sh
kubectl apply -f https://raw.githubusercontent.com/<org>/crd-test/<tag or branch>/dist/install.yaml
```

## Contributing
// TODO(user): Add detailed information on how you would like others to contribute to this project

**NOTE:** Run `make help` for more information on all potential `make` targets

More information can be found via the [Kubebuilder Documentation](https://book.kubebuilder.io/introduction.html)

## License

Copyright 2024.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

# crd-test步骤总结
## 1. 安装Operator SDK：
  - 根据你的操作系统，从Operator SDK GitHub页面下载并安装operator-sdk。
## 2. 初始化新项目：
  -  使用`operator-sdk init`命令初始化一个新的Operator项目，设置项目名称、Go模块名称、版本等。
## 3. 添加APIs服务：
  - 使用`operator-sdk create api`命令创建一个新的API服务，这将生成CRD定义和相关的Go代码。
## 4. 定义CRD：
  - 在生成的api/v1目录下，编辑types.go文件定义你的自定义资源类型和字段。
## 5. 生成CRD和代码：
  - 使用`make generate`命令生成CRD定义和相关的Go代码。
## 6. 编写业务逻辑：
  - 在`pkg/controller/<your-resource>`目录下，编写处理CRD实例的业务逻辑。
## 7. 构建Operator镜像：
  - 使用`operator-sdk build <IMAGE-NAME>`命令构建Operator的Docker镜像。
## 8. 推送镜像到容器仓库：
  - 将构建的镜像推送到Docker Hub或其他容器镜像仓库。
## 9. 部署CRD到Kubernetes：
  - 使用`kubectl apply -f deploy/crds`命令将CRD定义部署到Kubernetes集群。
## 10. 部署Operator：
  - 使用`kubectl apply -f deploy/operator.yaml`命令部署Operator到Kubernetes集群。
## 11. 配置RBAC：
  - 确保Operator具有适当的权限来管理CRD资源，这通常在`deploy/role.yaml和deploy/role_binding.yaml`中配置。
## 12. 创建CRD实例：
  - 使用`kubectl apply -f <crd-instance.yaml>`命令创建CRD的实例。
## 13. 验证Operator和CRD实例：
  - 使用`kubectl get <crd-name>`和`kubectl describe <crd-name> <instance-name>`命令检查CRD实例的状态。
## 14. 监控和调试：
  - 监控Operator的日志输出，使用`kubectl logs -f <pod-name>`命令查看实时日志。
## 15. 更新Operator：
  - 如果需要更新Operator或CRD定义，修改代码后重新构建和推送镜像，然后更新Kubernetes中的部署。
## 16. 清理资源：
  - 当不再需要CRD或Operator时，使用`kubectl delete -f deploy/crds`和`kubectl delete -f deploy/operator.yaml`命令进行清理。
