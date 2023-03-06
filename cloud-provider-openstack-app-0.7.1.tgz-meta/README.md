# How to install

## Modify values.yaml files

`emacs /helm/openstack-cinder-csi/values.yaml`

## Run helm command 

```
export KUBECONFIG=./<cluster>.kubeconfig 

helm install cloud-provider -n kube-system ./helm/cloud-provider-openstack-app/

```

# Metrics

https://github.com/kubernetes/cloud-provider-openstack/blob/master/docs/metrics.md

# Support cinder csi features :

https://github.com/kubernetes/cloud-provider-openstack/blob/master/docs/cinder-csi-plugin/using-cinder-csi-plugin.md#supported-features


# How to Update upstream charts 

These charts are located under helm/cloud-provider-openstack-app/charts/ and use git subtrees to track the changes.

* Please Note* git subtree uses git commit comments to track changes so a PR commit squash will break the below.


## Step 1

Navigate to https://github.com/giantswarm/cloud-provider-openstack/ and update the Giantswarm fork by clicking the fetch upstream button.

You also need to sync the tags on the fork.

```

git clone git@github.com:giantswarm/cloud-provider-openstack.git /tmp/cloud-provider-openstack

cd /tmp/cloud-provider-openstack

git remote add upstream git@github.com:kubernetes/cloud-provider-openstack.git

git fetch upstream

git push origin --tags

rm -rf /tmp/cloud-provider-openstack

```

## Step 2

```bash
export CLOUD_PROVIDER_VERSION=v1.23.1
export CHART_FOLDER=helm/cloud-provider-openstack-app/
./update_charts.sh --merge
```

## Step 2B ( if step 2 above failed )


You will need to reset the subtree because the commit message subtree is looking for is not present or overrriten in a squash.

```bash
export CLOUD_PROVIDER_VERSION=release-1.23
export CHART_FOLDER=helm/cloud-provider-openstack-app/

#Cleanup
git branch -D temp-split-branch
git remote remove upstream-copy

git rm -rf ${CHART_FOLDER}charts/openstack-cinder-csi
git rm -rf ${CHART_FOLDER}charts/openstack-cloud-controller-manager
git add ${CHART_FOLDER}charts/
git commit -m "Cleanup upstream chart folder"


./update-charts.sh --add
```

## Step 3

Push code to git repo

## Step 4

Merge PR without squash
