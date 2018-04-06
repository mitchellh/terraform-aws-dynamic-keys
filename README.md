AWS Dynamic Keypair
========================

This Terraform module dynamically generates an SSH key pair, imports it
into AWS, and provides the keypair via outputs.

This can be used to create AWS resources with a keypair that doesn't have
to exist prior to the Terraform run. This is great for demos! Warning that
the private key information will be in the Terraform state, so this isn't
well suited currently for production environments.

What This Module Does
---------------------

Features:

  * Creates a public/private keypair.

  * Writes the keypair into files (optional).

  * Imports the keypair into AWS.

  * Provides the OpenSSH-formatted public key and a PEM-encoded private
    key as output values that can be used directly with `connection` blocks.

Usage
-----

Run `terraform apply` against the example configuration below:

```hcl
provider "aws" {
  version = "~> 1.0.0"
  region  = "us-west-2"
}

module "keypair" {
  source = "mitchellh/dynamic-keys/aws"
  path   = "${path.root}/keys"
}

output "private_key_pem" {
  value = "${module.keypair.private_key_pem}"
}
```

The SSH keys created are also in the `./keys` subdirectory relative to
the root Terraform configuration if you need those for any other reason.
These keys will be automatically deleted and removed when your configuration
is destroyed (via `terraform destroy`).

Terraform version
-----------------

Terraform version 0.11.0 or newer is required for this version to work.
