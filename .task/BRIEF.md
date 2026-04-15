# Brief

## Feature
Reduzir o coldstart de tokens dos subagentes do plugin LSD de ~18k para menos de 5k tokens.

## Expected Behavior
Atualmente, quando o SKILL.md invoca um subagente (planner, executor, verifier), o Claude Code injeta automaticamente o contexto completo da sessão (CLAUDE.md, RTK.md, system prompts, READMEs, etc.) — totalizando ~18k tokens só para iniciar o subagente.

A solução é reformular os prompts dos agentes e/ou a forma como o SKILL.md os invoca, de modo que o agente principal passe ao subagente apenas o contexto mínimo necessário para ele executar sua tarefa. O subagente não deve receber conteúdo extra além do que o agente principal decide passar explicitamente.

## Constraints
- Liberdade total para reformular qualquer arquivo do skill (`SKILL.md`, `agents/*.md`)
- A semântica de cada passo (Plan, Execute, Verify) deve ser preservada
- Lean mode LEAN_CC ativo: os prompts dos subagentes devem ser projetados para rodar no Claude Code

## Done Criteria
- O usuário verifica o contador de tokens no terminal do Claude Code ao invocar `/lsd exec` (ou outro passo com subagente)
- Meta: coldstart < 5k tokens no primeiro turno do subagente
