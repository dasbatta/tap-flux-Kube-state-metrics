Monitoring  With Prometheus And Grafana

KSM is typically used to pull data from the status fields and specs of common kubernetes resources, and to convert them into metrics which can be scraped by prometheus.

KSM now includes the ability to define a config file with the desired mapping of resource fields to metric labels and values for any Custom Resources, and it will be able to provide us metrics for them as well!

With this in mind, I decided to perform a small POC of building out a monitoring suite for TAP, using KSM.

For this setup i decided to use the kube-prometheus-stack helm chart which makes deploying a fully functional Prometheus and Grafana stack as easy as it gets, and then extend the configuration to support the TAP CRDs, and generate the needed metrics.

For this initial POC i decided to focus on a subset of the TAP resources.

The resource types and corresponding prometheus metrics being monitored are:

workloads – cartographer_workload_info , cartographer_workload_status
deliverables – cartographer_deliverable_info , cartographer_deliverable_status
service bindings – service_binding_info, service_binding_status
cluster instance classes – stk_cluster_instance_class_composition_selector, stk_cluster_instance_class_status
class claims – stk_class_claim_info , stk_class_claim_status
resource claims – stk_resource_claim_info, stk_resource_claim_status
knative services – knative_service_info, knative_service_status
knative revisions – knative_revision_info, knative_revision_status
kapp controller package repositories – carvel_packagerepository_info
kapp controller package installations – carvel_packageinstall_info
kapp controller apps – carvel_app_info, carvel_app_namespaces
api descriptors – api_descriptor_info, api_descriptor_status
tekton pipeline runs – tekton_pipeline_run_info, tekton_pipeline_run_status
tekton task runs – tekton_task_run_info, tekton_task_run_status
accelerators – accelerator_info, accelerator_imports_info, accelerator_status
fragments – accelerator_fragment_info, accelerator_fragment_status
flux git repositories – flux_git_repository_info, flux_git_repository_status
image scans – scst_image_scan_info, scst_image_scan_status
source scans – scst_source_scan_info, scst_source_scan_status
kpack images – kpack_image_info, kpack_image_status
kpack builds – kpack_build_info, kpack_build_involved_buildpacks, kpack_build_status
With these resources and metrics defined I was able to create a simple yet very powerful dashboard in Grafana for visualizing the state of my TAP environment.

First we can show the status of out package installations

![Alt text](Dashboard_images/grafana-01.webp)
