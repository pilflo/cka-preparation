# Json Path

Kubernetes supports selecting specific fields from a json output using **JsonPath**.

## Syntax

A JsonPath query can be passed along any kubectl command using **-o=jsonpath='{*JSONPATHQUERY*}'**

```bash
kubectl get nodes -o=jsonpath='{.items[*].spec.taints[*]}'

kubectl get pods -o=jsonpath='{.items[0].spec.containers[0].image}'
```

You can concatenate the result of multiple jsonpath queries.
```bash
# Query1
kubectl get nodes -o=jsonpath='{.items[*].metadata.name}'
>> master node01
# Query2
kubectl get nodes -o=jsonpath='{.items[*].status.capacity.cpu}'
>> 4 4
# Query 1+2
kubectl get nodes -o=jsonpath='{.items[*].metadata.name}{.items[*].status.capacity.cpu}'
>> master node01 4 4
```

At this point the output is not pretty.


We can add *"\n"* and *"\t"* to add a bit of formatting.

```bash
kubectl get nodes -o=jsonpath='{.items[*].metadata.name}{"\n"}{.items[*].status.capacity.cpu}'
>> master node01 4 4
```

## Custom Columns

Custom Columns is a way to prettify our outputs and display relevant information.

```bash
# Format
kubectl get nodes -o=custom-columns=<COLUMN NAME>:<JSON PATH>

# Notice that the '{}' around jsonpath are not needed anymore. 
# Also the .items part of the query is useless since custom-columns already loops over items.
kubectl get nodes -o=custom-columns=NAME:.metadata.name
>> NAME
>> master
>> node01
# More columns can be added, separated with a comma ,
kubectl get nodes -o=custom-columns=NAME:.metadata.name,CPU:status.capacity.cpu
>> NAME    CPU
>> master   4
>> node01   4
```

## Sorting

You can sort outputs using **--sort-by=** flag.

```
kubectl get nodes --sort-by=.status.capacity.cpu
```






