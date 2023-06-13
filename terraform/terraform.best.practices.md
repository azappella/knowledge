# terraform best practices

- Terraform modules must follow the standard module structure.
- Start every module with a main.tf file, where resources are located by default.
- In every module, include a README.md file in Markdown format. In the README.md file, include basic documentation about the module.
- Place examples in an examples/ folder, with a separate subdirectory for each example. For each example, include a detailed README.md file.
- Create logical groupings of resources with their own files and descriptive names, such as network.tf, instances.tf, or loadbalancer.tf.
- Avoid giving every resource its own file. Group resources by their shared purpose. For example, combine google_dns_managed_zone and google_dns_record_set in dns.tf.
- In the module's root directory, include only Terraform (*.tf) and repository metadata files (such as README.md and CHANGELOG.md).
- Place any additional documentation in a docs/ subdirectory.


## References

- https://cloud.google.com/docs/terraform/best-practices-for-terraform