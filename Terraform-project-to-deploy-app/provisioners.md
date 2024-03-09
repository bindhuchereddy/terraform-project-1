Provisioners allow you to model specific actions on either the local machine or a remote machine during resource creation or destruction.
They enable additional configuration and setup tasks that can’t be accomplished with Terraform’s declarative syntax alone.
Types of Provisioners:
-----------------------------------
local-exec provisioner:
```````````````````````````
Invokes a local executable after a resource is created.
Executes a process on the machine running Terraform, not on the resource itself.
Useful for running scripts or commands locally.
Example usage:
---------------
resource "aws_instance" "example" {
  # ... other configuration ...

  provisioner "local-exec" {
    command = "echo 'Resource created!'"
  }
}

remote-exec provisioner:
`````````````````````````````````
Invokes a script on a remote resource after it is created.
Can be used for various tasks, such as running a configuration management tool or bootstrapping into a cluster.

Example usage:
---------------

resource "aws_instance" "example" {
  # ... other configuration ...

  provisioner "remote-exec" {
    inline = [
      "echo 'Resource created!'",
      "sudo apt-get update",
      "sudo apt-get install -y nginx",
    ]
  }
}
file Provisioner:
``````````````````````````````

The file provisioner is used to copy files or directories from the local machine to a remote machine. This is useful for deploying configuration files, scripts, or other assets to a provisioned instance.

Example:
---------------

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}

provisioner "file" {
  source      = "local/path/to/localfile.txt"
  destination = "/path/on/remote/instance/file.txt"
  connection {
    type     = "ssh"
    user     = "ec2-user"
    private_key = file("~/.ssh/id_rsa")
  }
}
In this example, the file provisioner copies the localfile.txt from the local machine to the /path/on/remote/instance/file.txt location on the AWS EC2 instance using an SSH connection.