# Fix stuck pvc in terminating state:

## Swap to your namespace

``` kubectl config set-context --current --namespace=<yournamespace> ```

## List your PVCs and see which one is stuck

``` kubectl get pvs ```

## See the finalizers for pvc

``` kubectl describe pvc <your-pvc-name> | grep Finalizers ```

## Set finalizer to null

``` kubectl patch pvc <your-pvc-name> -p '{"metadata":{"finalizers": []}}' --type=merge ```

## List your PVCs again

``` kubectl get pvs ```
