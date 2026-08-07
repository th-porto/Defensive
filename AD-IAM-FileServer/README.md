Documentação de um homelab completo com Active Directory, implementação de controle de acesso baseado em papéis (RBAC), compartilhamentos de arquivo com permissões em camadas e políticas de grupo (GPOs).

Objetivos:
•Estruturar Unidades Organizacionais (OUs) por departamento•
•Implementar controle de acesso baseado em papéis (RBAC) com grupos de segurança
•Configurar File Server com permissões em camadas (SMB + NTFS)
•Criar e aplicar Políticas de Grupo (GPOs) para padronização
•Validar e testar controle de acesso com diferentes níveis de privilégio

## Conceitos de Segurança Aplicados
•Princípio do Menor Privilégio: Usuários têm apenas as permissões necessárias
•Camadas de Permissão: SMB + NTFS = defesa em profundidade
•Separação de Responsabilidades: Admins de domínio vs HelpDesk
•Auditoria: Estrutura permite rastrear quem acessa o quê
•Gestão de Ciclo de Vida: Desabilitação de contas, mudança de departamento

Arquitetura:
Servidor AD (FS01)
├── Unidades Organizacionais (OUs)
│   ├── TI
│   ├── Financeiro
│   ├── RH
│   ├── Público
│   ├── Administração
│   └── Usuários Desabilitados
│
├── Grupos de Segurança (RBAC)
│   ├── gg-ti (acesso à pasta TI)
│   ├── gg-fin (acesso à pasta Financeiro)
│   ├── gg-rh (acesso à pasta RH)
│   ├── gg-admins-dominio (administradores)
│   └── gg-helpdesk-n1 (suporte com privilégios limitados)
│
├── Usuários (teste em cada departamento)
│   ├── thiago.silva (TI)
│   ├── usuario.fin (Financeiro)
│   └── usuario.rh (RH)
│
└── File Server (Z:)
    ├── Z:\Compartilhamentos\TI (acesso: gg-ti)
    ├── Z:\Compartilhamentos\Financeiro (acesso: gg-fin)
    ├── Z:\Compartilhamentos\RH (acesso: gg-rh)
    └── Z:\Compartilhamentos\Publico (acesso: todos - leitura)

Clientes Windows 11
└── Logados com usuários de diferentes departamentos

Vms usadas:
2 VMs: Windows Server (para AD) e Windows 11 (cliente)

