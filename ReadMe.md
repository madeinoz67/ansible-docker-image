# ansible docker image

Docker image to use [`ansible`](https://www.ansible.com/) to deploy using ssh in a CI :v:

[![Docker Stars](https://img.shields.io/docker/stars/gableroux/ansible.svg)](https://hub.docker.com/r/gableroux/ansible)
[![Docker Pulls](https://img.shields.io/docker/pulls/gableroux/ansible.svg)](https://hub.docker.com/r/gableroux/ansible)
[![Docker Automated](https://img.shields.io/docker/automated/gableroux/ansible.svg)](https://hub.docker.com/r/gableroux/ansible)
[![Docker Build](https://img.shields.io/docker/build/gableroux/ansible.svg)](https://hub.docker.com/r/gableroux/ansible)
[![Image](https://images.microbadger.com/badges/image/gableroux/ansible.svg)](https://microbadger.com/images/gableroux/ansible)
[![Version](https://images.microbadger.com/badges/version/gableroux/ansible.svg)](https://microbadger.com/images/gableroux/ansible)
[![Layers](https://images.microbadger.com/badges/image/gableroux/ansible.svg)](https://microbadger.com/images/gableroux/ansible)

## Usage

### Command line

```bash
docker run --rm -it gableroux/ansible:2.7.4 ansible --help
```

### gitlab-ci example

```yaml
.ansible: &ansible
  stage: deploy
  when: manual
  image: gableroux/ansible:2.7.4
  before_script:
    # https://docs.gitlab.com/ee/ci/ssh_keys/
    - eval $(ssh-agent -s)
    - mkdir ~/.ssh/
    - chmod 700 ~/.ssh
    - echo "$SSH_CONFIG" > ~/.ssh/config
    - echo "$SSH_KNOWN_HOSTS" > ~/.ssh/known_hosts
    - chmod 644 ~/.ssh/known_hosts
    - echo "$SSH_PRIVATE_KEY" | tr -d '\r' | ssh-add - > /dev/null

deploy-example-master:
  <<: *ansible
  script:
    - ansible --version
    - ansible-playbook -i host-example deploy.yml
  only:
    - master
```

## FAQ

### Why bother using a docker image

Installing with `pip` is fine, but pulling this image is faster.

### Why not use an official ansible image?

[As described here in the short description](https://store.docker.com/r/ansible/ansible):

> Images for automated testing of Ansible. They do not include Ansible and are not for end users. 

The official image is used to run tests for the ansible project. I wish they had and official image for actually running ansible.

### My version is not there, what can I do?

Fork the project, replace `ENV` and push your own image.

### Can I contribute?

Yes, why not?

### There are already a lot of ansible docker images out there, why a new one?

I don't trust people when it comes to running critical code against infrastructure. If you wish to use this, I recommend you to fork it and build your own.

## License

[MIT](LICENSE.md) © [Gabriel Le Breton](https://gableroux.com)