# torrust-installer
This is an Ansible-based installer for [Torrust](https://github.com/torrust/torrust). You can use it to set up a Torrust private tracker with minimal effort.

Here it is in action:

[![asciicast](https://asciinema.org/a/zlvMM1ehO1E7zFfwF6YAFWhWG.svg)](https://asciinema.org/a/zlvMM1ehO1E7zFfwF6YAFWhWG)

## Requirements

You will need the following:

* A computer with Ansible installed (hint: sudo apt install ansible or follow [these directions](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-specific-operating-systems)).
* A server you wish to install Torrust on, with a domain name pointed at the server via DNS.

## Getting started

Check out a copy of this repository.

```
git clone https://github.com/zorlin/torrust-installer
cd torrust-installer
```

Generate an SSH key if you don't already have one.

```
ssh-keygen
```

Copy your SSH key to your Torrust server (replace example.com with your server's address/domain name).

```
ssh-copy-id example.com
```

Add your Torrust server to the **inventory** file (replace example.com with your server's address/domain name).

```
echo example.com >> inventory/hosts
```

Create a "host variables folder" where you will set keys, passwords, URLs and other settings (again, replace example.com with your server's address/domain name).

```
mkdir inventory/host_vars/example.com
```

Create a variable file in that folder with all the basic configuration needed to start your tracker (paste this whole block, then open inventory/host_vars/example.com/vars.yml in an editor and change it as needed)

```
cat > inventory/host_vars/example.com/vars.yml << EOF
---
# Variables used for example.com

# Set URLs for the site and tracker
your_domain: "example.com"
# Leave this setting as-is to make your tracker use the same URL as your site.
tracker_url: "{{ your_domain }}"

# Used for internal authentication.
http_api_access_token: this-is-an-example-token-please-change-me
secret_key: this-is-an-example-secret-please-change-me

# Used to receive email alerts about expiring Let's Encrypt certificates.
certbot_email: example@protonmail.com
EOF
```

Now you're ready to rock! Run Ansible and watch as your tracker (hopefully) comes online automatically:

```
ansible-playbook site.yml
```

If the user you are connecting as requires a sudo password, run this command insatead.

```
ansible-playbook site.yml --ask-sudo-pass
```

You should see a message like this once Ansible finishes, indicating a successful run:

```
TASK [Success!] **********************************************************************************************
ok: [example.com] => {
    "msg": [
        "If you're reading this, Torrust has been successfully deployed to site example.com.",
        "Let us know if it worked, and feel free to reach out if you need help. Enjoy!"
    ]
}
```

You should now have a working tracker. Login and have a look around!

## Credits

torrust-installer was created by [Zorlin](https://github.com/zorlin/) and donated to the Torrust project in February 2022.
