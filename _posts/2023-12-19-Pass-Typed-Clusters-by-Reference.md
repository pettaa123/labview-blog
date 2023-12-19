---
title: "Pass Typed Clusters by Reference"
date: 2023-12-19
---
When creating SubVIs I like to have generic solutions that work for more than one strictly typed parameter. When dealing with structs, it's convenient to have them typed for ease of use. This simplifies propagating changes across your project with the simple act of pressing the Apply Changes button. The following snippet illustrates sending a typed cluster to a receiver that lacks knowledge about the cluster's specifics but is designed to process any received cluster.
![Bowtie](/labview-blog/assets/images/pass_different_cluster_by_reference.png)
