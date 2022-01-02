# Descomplicando Docker

### Docker Uncomplicated 


This repository is intended to save learning notes from Linuxtips Course of ["Descomplicando Docker"](https://www.linuxtips.io/products/descomplicando-o-docker)

Thanks a lot to [Jeferson](https://github.com/badtuxx) for this opportunity


## CREAING A LOCAL CLUSTER ENVIRONMENT
**Requirements:**
- docker
- vagrant

Run a `vagrant up` to setup a cluster with 1 manager / 2 workers (check `Vagrantfile` for details`)

Helpful vagrant commands:
- validate - Check file template
- init - prepare environment
- up - provision vms
- ssh <vm_name> - login at vm
- halt - shutdown vm
- resume <vm_name> - start vm
- destroy -f - delete all