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














from azure.devops.credentials import BasicAuthentication
from azure.devops.security.v6_0.models import AccessControlEntry, AccessControlList
from azure.devops.security.v6_0.security_client import SecurityClient

# Credenziali di autenticazione di base per l'API di DevOps
personal_access_token = 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
organization_url = 'https://dev.azure.com/organization'

credentials = BasicAuthentication('', personal_access_token)

# Creazione del client per l'API di sicurezza di DevOps
security_client = SecurityClient(base_url=organization_url, credentials=credentials)

# Recupero della access control list esistente
acl = security_client.get_access_control_list(
    namespace_id='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx',
    token='repoV2/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'
)

# Creazione della nuova entry di controllo accesso
new_ace = AccessControlEntry(
    descriptor={'identifier': 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'},
    allow_bits=7,
    deny_bits=0,
    extended_info=None
)

# Aggiunta della nuova entry alla access control list
acl.access_control_entries.append(new_ace)

# Aggiornamento della access control list sul server
updated_acl = security_client.set_access_control_list(
    namespace_id='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx',
    token='repoV2/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx',
    security_namespace_id='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx',
    security_token_id='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx',
    access_control_list=acl
)
