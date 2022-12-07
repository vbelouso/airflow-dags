# Only for test purpose

## Environment variables

```shell
export AIRFLOW_NAME="airflow-cluster"
export AIRFLOW_NAMESPACE="airflow"
```

## Helm repo configuration

```shell
helm repo add apache-airflow https://airflow.apache.org
```

## OpenShift SCC configuration

:information_source: No need for Kubernetes

```shell
oc new-project "${AIRFLOW_NAMESPACE}"
oc adm policy add-scc-to-user anyuid -z default -n "${AIRFLOW_NAMESPACE}"
```

## Airflow chart installation

### Installation without Git sync

The following command installs Airflow on `Openshift` with DAG examples and `KubernetesExecutor`

:information_source: For Kubernetes installation please remove parameter `--set rbac.createSCCRoleBinding=true`

```shell
helm upgrade --install airflow apache-airflow/airflow \
--namespace "${AIRFLOW_NAMESPACE}" \
--create-namespace \
--set rbac.createSCCRoleBinding=true \
--set executor=KubernetesExecutor \
--set-string 'env[0].name=AIRFLOW__CORE__LOAD_EXAMPLES,env[0].value=True'
```

### Installation with Git sync

The following command installs Airflow on `Openshift` with DAG examples, Git synchronization and `KubernetesExecutor`

:information_source: For Kubernetes installation please remove RBAC configuration from `custom-values.yml`

```shell
helm upgrade --install airflow apache-airflow/airflow \
--namespace "${AIRFLOW_NAMESPACE}" \
--create-namespace \
--values ./custom-values.yml
```
