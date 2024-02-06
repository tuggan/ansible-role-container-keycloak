container-keycloak
=========

Install and configure [keycloak](https://www.keycloak.org/) as a container.

Requirements
------------

A host running as a container host, currently only docker is supported.

Role Variables
--------------

| Variable                                    | Required | Default      | Description                                                               |
| ------------------------------------------- | -------- | ------------ | ------------------------------------------------------------------------- |
| container_keycloak_admin                    | No       | `omit`       | Initial admin user                                                        |
| container_keycloak_admin_password           | No       | `omit`       | Initial admin user password                                               |
| container_keycloak_db                       | Yes      |              | Database type                                                             |
| container_keycloak_db_url                   | Yes      |              | Database url, see example playbook                                        |
| container_keycloak_db_username              | Yes      |              | Database user                                                             |
| container_keycloak_db_password              | Yes      |              | Database password                                                         |
| container_keycloak_features                 | No       | `omit`       | Features to enable or disable                                             |
| container_keycloak_hostname                 | No       | `omit`       | KeyCloak hostname. Should be your fqdn                                    |
| container_keycloak_health_enabled           | No       | `true`       | Enable health endpoint                                                    |
| container_keycloak_metrics_enabled          | No       | `true`       | Enable metrics endpoint                                                   |
| container_keycloak_key_store_file           | No       | `omit`       | Path to key store file                                                    |
| container_keycloak_key_store_password       | No       | `omit`       | Key store file password                                                   |
| container_keycloak_proxy_mode               | No       | edge         | KeyCloak proxy mode. Edge is a good default when running through a proxy. |
| container_keycloak_container_labels         | No       | `omit`       | List of labels to assign the container. See example playbook              |
| container_keycloak_network                  | No       | See defaults | Network to connect the container to.                                      |
| container_keycloak_container_name           | No       | See defaults | Name of the container                                                     |
| container_keycloak_image                    | No       | See defaults | Container image to use                                                    |
| container_keycloak_tag                      | No       | See defaults | Container image tag                                                       |
| container_keycloak_container_restart_policy | No       | See defaults | Container restart policy                                                  |

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: ingress
      roles:
        - tuggan.container-keycloak
      vars:
        container_keycloak_network: apps
        container_keycloak_admin: admin
        container_keycloak_admin_password: "{{ secret_keycloak_admin_password }}"
        container_keycloak_hostname: auth.example.com
        container_keycloak_db: postgres
        container_keycloak_db_url: jdbc:postgresql://keycloak.ip/keycloak
        container_keycloak_db_username: keycloak
        container_keycloak_db_password: "{{ secret_keycloak_db_password }}"
        container_keycloak_container_labels:
          traefik.http.routers.keycloak-external.rule: Host(`auth.example.com`) && PathPrefix(`/js/`, `/realms/`, `/resources/`, `/robots.txt`)
          traefik.http.routers.keycloak-external.entrypoints: globalwebsecure
          traefik.http.routers.keycloak-internal.rule: Host(`auth.example.com`) 
          traefik.http.routers.keycloak-internal.entrypoints: websecure

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
