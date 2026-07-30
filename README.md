# powershell-windows-best-skill

> **Status: EXPERIMENTAL** — Esta skill está em fase ativa de testes e refinamento. Use por sua conta e risco. Sempre valide os scripts gerados antes de executá-los em ambientes críticos ou de produção.

Uma skill unificada do Claude Code para programação PowerShell **exclusivamente no Windows**. Consolida múltiplas skills da comunidade em uma única referência opinativa e otimizada para automação Windows com PowerShell 7+.

> 📖 **Leia em outro idioma:** [English / Inglês](README.en.md)

---

## O Que É Isso

`SKILL.md` é uma [skill do Claude Code](https://docs.anthropic.com/en/docs/claude-code) que fornece ao Claude conhecimento profundo e específico do Windows sobre PowerShell quando invocada. Cobre regras de sintaxe, tratamento de erros, padrões de performance, endurecimento de segurança, gerenciamento de módulos, testes e mais — sem ruído multiplataforma.

---

## Como Usar

### 1. Instalar a skill

Copie `SKILL.md` para o seu diretório de skills do Claude Code ou referencie-o diretamente a partir deste repositório.

### 2. Invocar em uma conversa

Referencie o arquivo da skill na sua sessão do Claude Code:

```
@SKILL.md escreva um script que monitore o espaço em disco e envie um alerta
```

Ou diga ao Claude para usá-la explicitamente:

```
Usando a skill powershell-windows, refatore este script para tratar erros corretamente
```

### 3. O que o Claude vai aplicar

Com a skill ativa, o Claude seguirá os padrões definidos no arquivo:

- Envolver cmdlets em parênteses ao usar operadores lógicos
- Usar apenas saída ASCII (sem emoji nos scripts)
- Verificar nulo antes de acessar propriedades
- Sempre especificar `-Depth` no `ConvertTo-Json`
- Usar `List[T]` em vez de `$array +=` para coleções dinâmicas
- Preferir `Join-Path` para construção de caminhos
- Incluir `#Requires`, `Set-StrictMode` e `$ErrorActionPreference` em todo script
- Aplicar atributos de validação de entrada em todos os parâmetros

---

## Arquivo da Skill

| Arquivo | Descrição |
|---------|-----------|
| `SKILL.md` | Skill unificada exclusiva para Windows (use esta) |

### Skills de Origem

A skill unificada foi construída consolidando e filtrando as seguintes skills da comunidade.

| Repositório | Escopo | Status |
|-------------|--------|--------|
| [sickn33/antigravity-awesome-skills](https://github.com/sickn99/antigravity-awesome-skills) · [skills.sh](https://www.skills.sh/sickn99/antigravity-awesome-skills/powershell-windows) | Pitfalls do Windows e regras críticas de sintaxe | Mesclado |
| [josiahsiegel/claude-plugin-marketplace](https://github.com/josiahsiegel/claude-plugin-marketplace) · [skills.sh](https://www.skills.sh/josiahsiegel/claude-plugin-marketplace/powershell-master) | Skill mestra completa de PowerShell — sintaxe, segurança, módulos | Mesclado (partes multiplataforma removidas) |
| [404kidwiz/claude-supercode-skills](https://github.com/404kidwiz/claude-supercode-skills) · [skills.sh](https://www.skills.sh/404kidwiz/claude-supercode-skills/powershell-7-expert) | Recursos do PS7+ | Parcialmente mesclado (apenas partes relevantes ao Windows) |

---

## O Que Foi Removido das Skills de Origem

O seguinte conteúdo foi intencionalmente excluído da skill unificada:

- Caminhos de instalação Linux e macOS
- Detecção de plataforma `$IsLinux`, `$IsMacOS`
- Remoting via SSH (alternativa multiplataforma ao WinRM)
- Orientação de "testar em todas as plataformas"
- Padrões de integração com Bash/shell
- Configurações de matriz multiplataforma em CI/CD
- Padrões de manipulação de caminhos multiplataforma

---

## Aviso

```
ESTA SKILL É FORNECIDA "NO ESTADO EM QUE SE ENCONTRA", SEM NENHUM TIPO DE GARANTIA.

- Scripts e padrões sugeridos por esta skill não foram validados
  em todos os ambientes, configurações ou versões do Windows.
- Sempre revise os scripts gerados antes de executá-los.
- Teste primeiro em um ambiente que não seja de produção.
- Nunca execute scripts não confiáveis com privilégios de administrador sem revisão.
- O autor não se responsabiliza por perda de dados, danos ao sistema
  ou qualquer outra consequência decorrente do uso desta skill.
```

**Antes de executar qualquer script em ambiente crítico:**

1. Leia o script inteiro e entenda o que ele faz
2. Execute `Invoke-ScriptAnalyzer -Path script.ps1` e corrija todas as finding
3. Teste com `-WhatIf` quando suportado
4. Teste em um ambiente de staging ou isolado primeiro
5. Garanta que você tenha um plano de rollback (backup, snapshot, etc.)

---

## Contribuindo

Encontrou um erro, padrão desatualizado ou conteúdo específico do Windows ausente? Abra uma issue ou envie um PR. Mantenha as contribuições exclusivas para Windows — conteúdo multiplataforma será rejeitado.