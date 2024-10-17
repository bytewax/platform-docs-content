Description of guide

## Steps

1. Deploy a dataflow with recovery enabled
```sh
waxctl df deploy simple_slow.py --platform --recovery=true
```

2. Show the state
https://dashboard.my-company.com/

3. Delete the dataflow
```sh
waxctl df rm --name bytewax --yes
```

4. Show that the dataflow is not anymore in the dashboard

https://dashboard.my-company.com/

5. Deploy the dataflow again
```sh
waxctl df deploy simple_slow.py --platform --recovery=true
```

6. Show the state again

https://dashboard.my-company.com/
