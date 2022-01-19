[![Docker Repository on Quay](https://quay.io/repository/loadwordteam/ghidra-server/status "Docker Repository on Quay")](https://quay.io/repository/loadwordteam/ghidra-server)

# Non-trivial Ghidra Server Container
My attempt to confine Ghidra server in a container for serious work
and not just as once in a time deployment. There are various reasons
why you want to start a small project on reverse engineering, our
software legacy spans decades and it's not unusual the need to
understand and modify a piece of machine code when the source is lost
or not available.

Ghidra has a nice feature for collaboration but needs a server
somewhere, the software it's a bit rough in my opinion but good
enough, needs a bit of care and patient to learn all the
quirks. Unfortunately doesn't play nice with RedHat based system,
that's why I wrote this Docekrfile.

![Some cool dragon illustration](https://github.com/Infrid/ghidra-server/raw/main/dragon.jpg)

## Getting Started
Read the files and the comments in the source code, this image is
useful especially if you want to deploy on a Fedora/Centos/RedHat
based system.

### Prerequisites
In order to run this container you'll need Docker installed but should
work just fine on podman, beware this server expect to run with root
privileges!


### Usage
Read the files in this repository, it's not hard to setup this service!

#### Environment Variables

Check the the `.env.example` file, you need a few things before
launching the service.

* `GHIDRA_CERT_PATH` A path you might want to mount as a volume for
  storing the SSL certificates
* `GHIDRA_CERT_PASSWORD` The password to unlock the certificate.

#### Volumes

* `/srv/repositories` - This directory holds your data when you work
  in ghidra, it's a good idea to backup it up.
  
#### Troubleshooting

##### 1 command(s) queued.

If you get this messace probably SELinux is holding your data from
docker, change the context to `svirt_sandbox_file_t` or similar.

##### Certificates do not conform to algorithm constraints

Usually, ghidra server creates a self-signed certificate with a very
weak algorithm, some gnu/Linux distribution like Fedora implements
some security policies for preventing you to connect to such "unsafe"
environments.

For this reason the `entrypoint.sh` file creates the certificates with
a stronger encryption, if you get this error check if you mounted the
`GHIDRA_CERT_PATH` as volume!

##### Failed to process keystore: keystore.jks

The server expects a keystore on start up, there is no automated
script to create a keystore on the fly on the first run.

You can force creation of a new keystore (and manage the users) by
running bash in the container `docker-compose run ghidra-server bash`.

## Authors

* **Gianluigi "Infrid" Cusimano** - *Initial work* and artist of
  making simple concept utterly complex.


[![a project by load word team](https://loadwordteam.com/logo-lwt-small.png "a project by load word team")](https://loadwordteam.com)


## License

This project is licensed under the MIT License - see the
[LICENSE](LICENSE) file for details.

## Acknowledgements

Thanks to [Giovanni "gionniboy" Pullara](https://github.com/gionniboy)
and Filippo "joeyrs" Civiletti for the patience and kindness in
answering all my questions and point me in the right direction.

Thanks to [Ash](https://github.com/QuarkTheAwesome) for writing about
the certificates used by ghidra on [his website](https://heyquark.com/sysadmin/2020/11/14/ghidra-tls/).

Image source available on [*Old book illustartions*](https://www.oldbookillustrations.com/illustrations/story-susa/)

