# example2
test

from azure.devops.connection import Connection
from msrest.authentication import BasicAuthentication
from azure.devops.v6_0.security import SecurityClient
from azure.devops.v6_0.security.models import (
    AccessControlEntry,
    AzureGroupProvider,
    Permission,
    SecurityNamespace,
    TokenScope
)

# definire la connessione a Azure DevOps
personal_access_token = 'PAT'
organization_url = 'https://dev.azure.com/OrganizationName/'
project_name = 'ProjectName'
repository_name = 'RepositoryName'
branch_name = 'BranchName'

credentials = BasicAuthentication('', personal_access_token)
connection = Connection(base_url=organization_url, creds=credentials)

# creare un oggetto SecurityClient per gestire i permessi di sicurezza
security_client = connection.clients_v6_0.get_security_client()

# definire l'utente a cui si vogliono concedere i permessi di scrittura
user_id = 'userid@organizationname.com'

# definire l'AccessControlEntry per assegnare i permessi di scrittura all'utente specifico
write_permission = Permission(bit=4, name='write')
ace = AccessControlEntry(
    descriptor=AzureGroupProvider(subject_descriptor=user_id),
    allow_bits=write_permission,
    deny_bits=Permission(),
    extended_info=None
)

# definire il SecurityNamespace per i permessi di sicurezza sulla repository e sulla branch specifica
security_namespace = SecurityNamespace(
    actions=None,
    dataspace_category=None,
    dataspace_name=None,
    name='repoV2/' + project_name + '/' + repository_name + '/' + branch_name,
    read_permission=Permission(),
    token_scope=TokenScope.organization_level,
    write_permission=Permission()
)

# aggiungere l'AccessControlEntry al SecurityNamespace
security_client.set_access_control_entries(entries=[ace], security_namespace_id=security_namespace.id)
