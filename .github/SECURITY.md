# Política de Segurança

## Reportando uma Vulnerabilidade

Se você descobrir uma vulnerabilidade de segurança neste repositório, **não abra uma issue pública**.

Em vez disso, reporte de forma privada por um dos canais abaixo:

- **GitHub Security Advisory (recomendado):** aba *Security* do repositório → *Report a vulnerability*
- **E-mail proprietário:** abra uma DM no GitHub ou use o contato do perfil [@raphaol](https://github.com/raphaol)

Por favor inclua no relatório:

1. Descrição clara da vulnerabilidade e do impacto potencial
2. Passos para reproduzir ou PoC
3. Versões/branches afetadas
4. Sugestão de mitigação, se houver

## Tempo de Resposta Esperado

| Etapa | Prazo |
|-------|-------|
| Confirmação de recebimento | até 72 horas |
| Avaliação inicial e classificação de severidade | até 7 dias |
| Correção ou mitigação publicada | até 30 dias para alta/crítica, 90 dias para média/baixa |

Será informado um ID para acompanhar o status enquanto a issue estiver aberta.

## Escopo

Esta política cobre apenas o conteúdo deste repositório (`powershell-windows-best-skill`), que consiste unicamente no arquivo de skill `SKILL.md`, documentação e configurações do repo.

Scripts PowerShell *gerados* pela skill durante o uso não estão no escopo — estes são de responsabilidade de quem os gera e executa. Antes de usar qualquer script gerado, siga o checklist do [README](README.md) (`Invoke-ScriptAnalyzer`, `-WhatIf`, ambiente de staging, plano de rollback).

## Medidas de Segurança Ativas no Repositório

- ✅ Secret scanning e push protection habilitados
- ✅ Branch `main` protegida (exige revisão de PR)
- ✅ Branches temporárias excluídas automaticamente após merge
- ✅ Vulnerability alerts e Dependabot security updates habilitados

## Fora do Escopo

- Vulnerabilidades em dependências de terceiros não presentes neste repo (reporte ao mantenedor do projeto upstream)
- Ataques que requeiram privilégios administrativos no Windows já comprometidos
- Conteúdo de skills de origem (repositórios `sickn99/antigravity-awesome-skills`, `josiahsiegel/claude-plugin-marketplace`, `404kidwiz/claude-supercode-skills`)

## Práticas Seguras Recomendadas para Uso da Skill

```
ESTA SKILL É FORNECIDA "NO ESTADO EM QUE SE ENCONTRA", SEM NENHUM TIPO DE GARANTIA.

- Sempre revise scripts gerados antes de executá-los
- Nunca execute scripts não confiáveis com privilégios de administrador
- Teste em ambiente isolado primeiro
- O autor não se responsabiliza por perda de dados ou danos ao sistema
```

## Reconhecimento

Agradecemos aos pesquisadores que relatam vulnerabilizes de forma responsável. Contribuições de segurança reconhecidas podem ser listadas aqui mediante consentimento do autor.