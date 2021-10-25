# flux-bug-test

## label-length

demonstrate how kustomizations with long names fail to recreate resources

to configure

 - select your local test cluster (minikube, kind etc.)
 - install flux with `flux install`
 - install the test:

    ```shell
    $ kubectl apply -f label-length/setup/
    gitrepository.source.toolkit.fluxcd.io/flux-bug-test created
    kustomization.kustomize.toolkit.fluxcd.io/long-name-which-goes-on-and-on-and-on-until-it-exceeds-63-characters created
    kustomization.kustomize.toolkit.fluxcd.io/short-name created
    ```

- observe the issue that kustomizations with names longer than 63 characters cannot create any resources, because they try to label them with a value equal to `metadata.name` which is too long:

    ```shell
    $ kubectl get kustomization -A
    NAMESPACE     NAME                                                                   READY   STATUS                                                                                                                                                                                                                                     AGE
    flux-system   long-name-which-goes-on-and-on-and-on-until-it-exceeds-63-characters   False   Namespace/test-long dry-run failed, reason: Invalid, error: Namespace "test-long" is invalid: metadata.labels: Invalid value: "long-name-which-goes-on-and-on-and-on-until-it-exceeds-63-characters": must be no more than 63 characters   36s
    flux-system   short-name                                                             True    Applied revision: main/e54653da5ce56ecc37a12c77f97f6020cc858b13
    ```


