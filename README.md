# Helm Operator 설치 가이드

## 구성 요소 및 버전
* Helm-operator (docker.io/fluxcd/helm-operator:1.2.0)

## Prerequisites

## 폐쇄망 구축 가이드
설치를 진행하기 전 아래의 과정을 통해 필요한 이미지 및 yaml 파일을 준비한다.
1. **폐쇄망에서 설치하는 경우** 사용하는 image repository에 helm-operator 설치 시 필요한 이미지를 push한다.
   * [install-registry 이미지 푸시하기 참조](https://github.com/tmax-cloud/install-registry/blob/5.0/podman.md)
2. install yaml을 다운로드한다.
    ```bash
    wget https://raw.githubusercontent.com/tmax-cloud/install-helm-operator/5.0/manifest/crds.yaml
    wget https://raw.githubusercontent.com/tmax-cloud/install-helm-operator/5.0/manifest/rbac.yaml
    wget https://raw.githubusercontent.com/tmax-cloud/install-helm-operator/5.0/manifest/deployment.yaml
    ```

## 설치 가이드
1. [Namespace 생성](#Step-1-Namespace-생성)
2. [CRD 배포](#Step-2-CRD-배포)
3. [RBAC 설정](#Step-3-RBAC-설정)
4. [Helm Operator 설치](#Step-4-Helm-Operator-설치)

## Step 1. Namespace 생성
* 목적 : `Helm Operator가 설치될 namespace 생성`
* 생성 순서 : 아래 command로 namespace 생성
	```bash
    kubectl create namespace helm-ns
	```

## Step 2. CRD 배포
* 목적 : `HelmRelease CRD 배포`
* 생성 순서 : 아래 command로 CRD 배포
	```bash
    kubectl apply -f crds.yaml
	```

## Step 3. RBAC 설정
* 목적 : `Helm Operator에 필요한 권한 부여`
* 생성 순서 : 아래 command로 RBAC 설정
	```bash
    kubectl apply -f rbac.yaml
	```

## Step 4. Helm Operator 설치
* 목적 : `Helm Operator 설치`
* 생성 순서 : 아래 command로 deployment 생성
	```bash
    kubectl apply -f deployment.yaml
	```
* 생성 순서 : 아래 command로 deployment 생성

## 설치 확인
1. [Pod 상태 확인](#Step-1-Pod-상태-확인)

## Step 1. Pod 상태 확인
* 목적 : `Helm Operator 정상 기동 확인`
* 생성 순서 : 아래 command로 Pod 상태가 running인지 확인
	```console
	$ kubectl get pods -n helm-ns
    NAME                               READY   STATUS    RESTARTS   AGE
    helm-operator-79f99c9df5-xpckn     1/1     Running   0          11s
  ```

## 삭제 가이드
1. [Helm Operator 리소스 제거](#Step-1-Helm-Operator-리소스-제거)

## Step 1. Helm Operator 리소스 제거
* 목적 : `Helm Operator 관련 리소스 제거`
* 삭제 순서 : 아래 command로 리소스 제거
	```bash
    kubectl delete -f deployment.yaml
    kubectl delete -f rbac.yaml
    kubectl delete -f crds.yaml
    kubectl delete namespace helm-ns
	```
