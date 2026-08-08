Documentação de um homelab completo com Active Directory, implementação de controle de acesso baseado em papéis (RBAC), compartilhamentos de arquivo com permissões em camadas e políticas de grupo (GPOs).

## Objetivos:

•Estruturar Unidades Organizacionais (OUs) por departamento•
•Implementar controle de acesso baseado em papéis (RBAC) com grupos de segurança
•Configurar File Server com permissões em camadas (SMB + NTFS)
•Criar e aplicar Políticas de Grupo (GPOs) para padronização
•Validar e testar controle de acesso com diferentes níveis de privilégio

---

## Conceitos de Segurança Aplicados

•Princípio do Menor Privilégio: Usuários têm apenas as permissões necessárias

•Camadas de Permissão: SMB + NTFS = defesa em profundidade

•Separação de Responsabilidades: Admins de domínio vs HelpDesk

•Auditoria: Estrutura permite rastrear quem acessa o quê

•Gestão de Ciclo de Vida: Desabilitação de contas, mudança de departamento

---

## Arquitetura:

```text
Domínio: corp.laboratorio.teste
│
├── DC01 — Controlador de Domínio
│   └── OU: EMPRESA
│       ├── Usuarios
│       │   ├── TI
│       │   │   └── Thiago Silva (thiago.silva)
│       │   ├── Financeiro
│       │   ├── RH
│       │   ├── Suporte
│       │   └── Desabilitados
│       │
│       ├── Computadores
│       │   ├── WorkStations
│       │   │   └── CORP\_CLIENTE — Windows 11
│       │   └── Quarentena
│       │
│       ├── Servidores
│       │   ├── Arquivos
│       │   │   └── FS01 — File Server
│       │   ├── Aplicacoes
│       │   └── Monitoramento
│       │
│       ├── Grupos
│       │   ├── Globais
│       │   │   ├── GG-TI
│       │   │   ├── GG-FIN
│       │   │   ├── GG-RH
│       │   │   └── GG-SUPORTE
│       │   │
│       │   ├── Recursos
│       │   │   ├── PASTA-TI-RW
│       │   │   ├── PASTA-FIN-RW
│       │   │   ├── PASTA-RH-RW
│       │   │   └── PASTA-PUBLICA-R
│       │   │
│       │   └── Administrativos
│       │       ├── GG-ADMINS-DOMINIO → membro de Domain Admins
│       │       └── GG-HELPDESK-N1 → gerenciamento limitado de usuários e senhas
│       │
│       ├── Administracao
│       │   ├── Admins
│       │   └── HelpDesk
│       │
│       └── Contas-de-Servico
│
├── FS01 — Servidor de Arquivos
│   └── Z:\Compartilhamentos
│       ├── TI          → \FS01\TI
│       ├── Financeiro → \FS01\Financeiro
│       ├── RH          → \FS01\RH
│       └── Publico     → \FS01\Publico
│
├── Fluxo de acesso
│   ├── Usuário de TI → GG-TI → PASTA-TI-RW → pasta TI
│   ├── Usuário do Financeiro → GG-FIN → PASTA-FIN-RW → pasta Financeiro
│   ├── Usuário de RH → GG-RH → PASTA-RH-RW → pasta RH
│   └── PASTA-PUBLICA-R → pasta Publico com acesso somente de leitura
│
└── GPOs
├── GPO-USUARIOS-BLOQUEIO-DE-TELA
│   └── Aplicada em EMPRESA\Usuarios
└── GPO-MAPEAR-PASTA-TI
└── Aplicada em EMPRESA\Usuarios\TI
└── Mapeia T: para \FS01\TI
```

---

## Vms usadas:

2 VMs: Windows Server (para AD) e Windows 11 (cliente)
