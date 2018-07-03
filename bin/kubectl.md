# kubectl

## version

    kubectl version

## information about the cluster

    kubectl cluster-info

## set different current-context (taken from kubeconfig file)

    kubectl config use-context CONTEXT_NAME

## get status of different internal components

    kubectl get componentstatuses

## get pods

we include all namespaces because by default kubectl will only show
the default namespace)

    kubectl get pods --all-namespaces

## get services

    kubectl get services --all-namespaces

## view config

    kubectl config view