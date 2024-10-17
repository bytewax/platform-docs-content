## API Reference

Packages:

- [bytewax.io/v1alpha1](#bytewaxiov1alpha1)

## bytewax.io/v1alpha1

Resource Types:

- [Dataflow](#dataflow)




## Dataflow
<sup><sup>[↩ Parent](#bytewaxiov1alpha1 )</sup></sup>






Dataflow is the Schema for the dataflows API

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
      <td><b>apiVersion</b></td>
      <td>string</td>
      <td>bytewax.io/v1alpha1</td>
      <td>true</td>
      </tr>
      <tr>
      <td><b>kind</b></td>
      <td>string</td>
      <td>Dataflow</td>
      <td>true</td>
      </tr>
      <tr>
      <td><b><a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.27/#objectmeta-v1-meta">metadata</a></b></td>
      <td>object</td>
      <td>Refer to the Kubernetes API documentation for the fields of the `metadata` field.</td>
      <td>true</td>
      </tr><tr>
        <td><b><a href="#dataflowspec">spec</a></b></td>
        <td>object</td>
        <td>
          DataflowSpec defines the desired state of Dataflow<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b><a href="#dataflowstatus">status</a></b></td>
        <td>object</td>
        <td>
          DataflowStatus defines the observed state of Dataflow<br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Dataflow.spec
<sup><sup>[↩ Parent](#dataflow)</sup></sup>



DataflowSpec defines the desired state of Dataflow

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b><a href="#dataflowspecimage">image</a></b></td>
        <td>object</td>
        <td>
          Dataflow container image settings<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>pythonFileName</b></td>
        <td>string</td>
        <td>
          Python script file to run<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>chartValues</b></td>
        <td>string</td>
        <td>
          Advanced Bytewax helm chart values. See more at https://bytewax.github.io/helm-charts<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>concurrencyPolicy</b></td>
        <td>string</td>
        <td>
          Specifies how to treat concurrent executions of a Scheduled Dataflow. Valid values are: - "Allow": allows Dataflows to run concurrently; - "Forbid": (default) forbids concurrent runs, skipping next run if previous run hasn't finished yet; - "Replace": cancels currently running Dataflow and replaces it with a new one<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>configMapName</b></td>
        <td>string</td>
        <td>
          Dataflow Configmap name<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>dependencies</b></td>
        <td>[]string</td>
        <td>
          Python dependencies needed to run the dataflow (use pip syntax, e.g. package-name==0.1.0)<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b><a href="#dataflowspecenvindex">env</a></b></td>
        <td>[]object</td>
        <td>
          Environment variables to inject to dataflow container<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>jobMode</b></td>
        <td>boolean</td>
        <td>
          Run a Job in Kubernetes instead of an Statefulset. Use this to batch processing<br/>
          <br/>
            <i>Default</i>: false<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>keepAlive</b></td>
        <td>boolean</td>
        <td>
          Keep the Dataflow container alive after dataflow ends. It could be useful for troubleshooting purposes<br/>
          <br/>
            <i>Default</i>: false<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>processesCount</b></td>
        <td>integer</td>
        <td>
          Number of processes to run<br/>
          <br/>
            <i>Default</i>: 1<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b><a href="#dataflowspecrecovery">recovery</a></b></td>
        <td>object</td>
        <td>
          Stores recovery files in Kubernetes persistent volumes to allow resuming after a restart (your dataflow must have recovery enabled: https://bytewax.io/docs/getting-started/recovery)<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>schedule</b></td>
        <td>string</td>
        <td>
          Dataflow schedule in Cron format, see https://en.wikipedia.org/wiki/Cron<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>suspend</b></td>
        <td>boolean</td>
        <td>
          Suspends the Dataflow execution. For Scheduled Dataflows, it will suspend subsequent executions, it does not apply to already started executions.<br/>
          <br/>
            <i>Default</i>: false<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>tarName</b></td>
        <td>string</td>
        <td>
          Tar file name stored in the dataflow configmap<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>workersPerProcess</b></td>
        <td>integer</td>
        <td>
          Number of workers to run in each process<br/>
          <br/>
            <i>Default</i>: 1<br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Dataflow.spec.image
<sup><sup>[↩ Parent](#dataflowspec)</sup></sup>



Dataflow container image settings

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b>tag</b></td>
        <td>string</td>
        <td>
          Container image tag<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>pullPolicy</b></td>
        <td>string</td>
        <td>
          Container image pull policy (the value must be Always, IfNotPresent or Never)<br/>
          <br/>
            <i>Default</i>: Always<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>pullSecret</b></td>
        <td>string</td>
        <td>
          Kubernetes secret name to pull images<br/>
          <br/>
            <i>Default</i>: default-credentials<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>repository</b></td>
        <td>string</td>
        <td>
          Container image repository URI<br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Dataflow.spec.env[index]
<sup><sup>[↩ Parent](#dataflowspec)</sup></sup>





<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b>name</b></td>
        <td>string</td>
        <td>
          Environment variable name<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>value</b></td>
        <td>string</td>
        <td>
          Environment variable value<br/>
        </td>
        <td>true</td>
      </tr></tbody>
</table>


### Dataflow.spec.recovery
<sup><sup>[↩ Parent](#dataflowspec)</sup></sup>



Stores recovery files in Kubernetes persistent volumes to allow resuming after a restart (your dataflow must have recovery enabled: https://bytewax.io/docs/getting-started/recovery)

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b>backupInterval</b></td>
        <td>integer</td>
        <td>
          System time duration in seconds to keep extra state snapshots around<br/>
          <br/>
            <i>Default</i>: 1<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b><a href="#dataflowspecrecoverycloudbackup">cloudBackup</a></b></td>
        <td>object</td>
        <td>
          Back up worker state DBs to cloud storage<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>enabled</b></td>
        <td>boolean</td>
        <td>
          Enable Dataflow recovery feature<br/>
          <br/>
            <i>Default</i>: false<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>partsCount</b></td>
        <td>integer</td>
        <td>
          Number of recovery partitions<br/>
          <br/>
            <i>Default</i>: 1<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b><a href="#dataflowspecrecoverypersistence">persistence</a></b></td>
        <td>object</td>
        <td>
          Kubernetes Persistence settings<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>singleVolume</b></td>
        <td>boolean</td>
        <td>
          Use only one persistent volume for all dataflow's pods in Kubernetes<br/>
          <br/>
            <i>Default</i>: false<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>snapshotInterval</b></td>
        <td>integer</td>
        <td>
          System time duration in seconds to snapshot state for recovery<br/>
          <br/>
            <i>Default</i>: 1<br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Dataflow.spec.recovery.cloudBackup
<sup><sup>[↩ Parent](#dataflowspecrecovery)</sup></sup>



Back up worker state DBs to cloud storage

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b>enabled</b></td>
        <td>boolean</td>
        <td>
          Enables Cloud Backup feature<br/>
          <br/>
            <i>Default</i>: false<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b><a href="#dataflowspecrecoverycloudbackups3">s3</a></b></td>
        <td>object</td>
        <td>
          Cloud Backup S3 settings<br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Dataflow.spec.recovery.cloudBackup.s3
<sup><sup>[↩ Parent](#dataflowspecrecoverycloudbackup)</sup></sup>



Cloud Backup S3 settings

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b>url</b></td>
        <td>string</td>
        <td>
          S3 url to store state backups. For example, s3://mybucket/mydataflow-state-backups<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>accessKeyId</b></td>
        <td>string</td>
        <td>
          AWS credentials access key id<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>secretAccessKey</b></td>
        <td>string</td>
        <td>
          AWS credentials secret access key<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>secretName</b></td>
        <td>string</td>
        <td>
          Name of the Kubernetes secret storing AWS credentials<br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Dataflow.spec.recovery.persistence
<sup><sup>[↩ Parent](#dataflowspecrecovery)</sup></sup>



Kubernetes Persistence settings

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b>size</b></td>
        <td>string</td>
        <td>
          Size of the persistent volume claim to be assign to each dataflow pod in Kubernetes<br/>
          <br/>
            <i>Default</i>: 10Gi<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>storageClassName</b></td>
        <td>string</td>
        <td>
          Storage class of the persistent volume claim to be assign to each dataflow pod in Kubernetes<br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Dataflow.status
<sup><sup>[↩ Parent](#dataflow)</sup></sup>



DataflowStatus defines the observed state of Dataflow

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b><a href="#dataflowstatusconditionsindex">conditions</a></b></td>
        <td>[]object</td>
        <td>
          INSERT ADDITIONAL STATUS FIELD - define observed state of cluster Important: Run "make" to regenerate code after modifying this file<br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Dataflow.status.conditions[index]
<sup><sup>[↩ Parent](#dataflowstatus)</sup></sup>



Condition contains details for one aspect of the current state of this API Resource. --- This struct is intended for direct use as an array at the field path .status.conditions.  For example, 
 type FooStatus struct{ // Represents the observations of a foo's current state. // Known .status.conditions.type are: "Available", "Progressing", and "Degraded" // +patchMergeKey=type // +patchStrategy=merge // +listType=map // +listMapKey=type Conditions []metav1.Condition `json:"conditions,omitempty" patchStrategy:"merge" patchMergeKey:"type" protobuf:"bytes,1,rep,name=conditions"` 
 // other fields }

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b>lastTransitionTime</b></td>
        <td>string</td>
        <td>
          lastTransitionTime is the last time the condition transitioned from one status to another. This should be when the underlying condition changed.  If that is not known, then using the time when the API field changed is acceptable.<br/>
          <br/>
            <i>Format</i>: date-time<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>message</b></td>
        <td>string</td>
        <td>
          message is a human readable message indicating details about the transition. This may be an empty string.<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>reason</b></td>
        <td>string</td>
        <td>
          reason contains a programmatic identifier indicating the reason for the condition's last transition. Producers of specific condition types may define expected values and meanings for this field, and whether the values are considered a guaranteed API. The value should be a CamelCase string. This field may not be empty.<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>status</b></td>
        <td>enum</td>
        <td>
          status of the condition, one of True, False, Unknown.<br/>
          <br/>
            <i>Enum</i>: True, False, Unknown<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>type</b></td>
        <td>string</td>
        <td>
          type of condition in CamelCase or in foo.example.com/CamelCase. --- Many .condition.type values are consistent across resources like Available, but because arbitrary conditions can be useful (see .node.status.conditions), the ability to deconflict is important. The regex it matches is (dns1123SubdomainFmt/)?(qualifiedNameFmt)<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>observedGeneration</b></td>
        <td>integer</td>
        <td>
          observedGeneration represents the .metadata.generation that the condition was set based upon. For instance, if .metadata.generation is currently 12, but the .status.conditions[x].observedGeneration is 9, the condition is out of date with respect to the current state of the instance.<br/>
          <br/>
            <i>Format</i>: int64<br/>
            <i>Minimum</i>: 0<br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>