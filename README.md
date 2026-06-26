# Coding Agent Skills

Colección de skills para asistentes de desarrollo como Claude Code y Codex CLI, orientadas a flujos de trabajo más seguros.

## Skills disponibles

### requirements-clarifier

Skill para evitar cambios prematuros o mal entendidos en el código.

Antes de modificar archivos, ayuda al agente a:

- Analizar la solicitud.
- Clasificar el nivel de riesgo.
- Revisar archivos relevantes cuando sea necesario.
- Proponer un plan mínimo seguro.
- Definir criterios de aceptación.
- Pedir aprobación antes de modificar archivos.

La skill permite cambios triviales directos solo cuando son pequeños, reversibles y claramente especificados.

## Instalación en Claude Code

### Windows PowerShell

Ejecuta este comando:

```powershell
$skillDir = "$HOME\.claude\skills\requirements-clarifier"
New-Item -ItemType Directory -Force -Path $skillDir | Out-Null

Invoke-WebRequest `
  -Uri "https://raw.githubusercontent.com/JEHS96/coding-agent-skills/main/requirements-clarifier/SKILL.md" `
  -OutFile "$skillDir\SKILL.md"
```

Luego abre Claude Code:

```powershell
claude
```

Y prueba la skill:

```text
/requirements-clarifier
```

### macOS/Linux

Ejecuta:

```bash
mkdir -p ~/.claude/skills/requirements-clarifier

curl -fsSL \
  https://raw.githubusercontent.com/JEHS96/coding-agent-skills/main/requirements-clarifier/SKILL.md \
  -o ~/.claude/skills/requirements-clarifier/SKILL.md
```

Luego abre Claude Code:

```bash
claude
```

Y prueba la skill:

```text
/requirements-clarifier
```

## Instalación en Codex CLI

### Windows PowerShell

Ejecuta este comando:

```powershell
$skillDir = "$HOME\.codex\skills\requirements-clarifier"
New-Item -ItemType Directory -Force -Path $skillDir | Out-Null

Invoke-WebRequest `
  -Uri "https://raw.githubusercontent.com/JEHS96/coding-agent-skills/main/requirements-clarifier/SKILL.md" `
  -OutFile "$skillDir\SKILL.md"
```

Luego reinicia Codex CLI:

```powershell
codex
```

Uso recomendado:

```text
Antes de modificar archivos, usa la skill requirements-clarifier.
Analiza primero, propón un plan y espera mi aprobación.
```

También se recomienda usar Codex en modo restrictivo para evitar cambios automáticos:

```text
/permissions
```

Selecciona:

```text
Read Only
```

O configura Codex con:

```toml
approval_policy = "on-request"
sandbox_mode = "read-only"
```

### macOS/Linux

Ejecuta:

```bash
mkdir -p ~/.codex/skills/requirements-clarifier

curl -fsSL \
  https://raw.githubusercontent.com/JEHS96/coding-agent-skills/main/requirements-clarifier/SKILL.md \
  -o ~/.codex/skills/requirements-clarifier/SKILL.md
```

Luego abre Codex CLI:

```bash
codex
```

## Instalación clonando el repositorio

También puedes instalar la skill clonando este repositorio.

### Claude Code en macOS/Linux

```bash
git clone https://github.com/JEHS96/coding-agent-skills.git
mkdir -p ~/.claude/skills
cp -R coding-agent-skills/requirements-clarifier ~/.claude/skills/requirements-clarifier
```

### Claude Code en Windows PowerShell

```powershell
git clone https://github.com/JEHS96/coding-agent-skills.git
New-Item -ItemType Directory -Force -Path "$HOME\.claude\skills" | Out-Null
Copy-Item -Recurse ".\coding-agent-skills\requirements-clarifier" "$HOME\.claude\skills\requirements-clarifier" -Force
```

### Codex CLI en macOS/Linux

```bash
git clone https://github.com/JEHS96/coding-agent-skills.git
mkdir -p ~/.codex/skills
cp -R coding-agent-skills/requirements-clarifier ~/.codex/skills/requirements-clarifier
```

### Codex CLI en Windows PowerShell

```powershell
git clone https://github.com/JEHS96/coding-agent-skills.git
New-Item -ItemType Directory -Force -Path "$HOME\.codex\skills" | Out-Null
Copy-Item -Recurse ".\coding-agent-skills\requirements-clarifier" "$HOME\.codex\skills\requirements-clarifier" -Force
```

## Actualizar la skill

### Claude Code en Windows PowerShell

```powershell
Invoke-WebRequest `
  -Uri "https://raw.githubusercontent.com/JEHS96/coding-agent-skills/main/requirements-clarifier/SKILL.md" `
  -OutFile "$HOME\.claude\skills\requirements-clarifier\SKILL.md"
```

### Claude Code en macOS/Linux

```bash
curl -fsSL \
  https://raw.githubusercontent.com/JEHS96/coding-agent-skills/main/requirements-clarifier/SKILL.md \
  -o ~/.claude/skills/requirements-clarifier/SKILL.md
```

### Codex CLI en Windows PowerShell

```powershell
Invoke-WebRequest `
  -Uri "https://raw.githubusercontent.com/JEHS96/coding-agent-skills/main/requirements-clarifier/SKILL.md" `
  -OutFile "$HOME\.codex\skills\requirements-clarifier\SKILL.md"
```

### Codex CLI en macOS/Linux

```bash
curl -fsSL \
  https://raw.githubusercontent.com/JEHS96/coding-agent-skills/main/requirements-clarifier/SKILL.md \
  -o ~/.codex/skills/requirements-clarifier/SKILL.md
```

## Uso recomendado

Puedes invocarla directamente dentro del agente:

```text
/requirements-clarifier
```

También se activa ante solicitudes como:

```text
Implementa este cambio
Corrige este bug
Refactoriza este módulo
Agrega esta validación
Cambia esta regla de negocio
Integra este endpoint
```

Para reforzar el comportamiento, puedes iniciar tus solicitudes con:

```text
Antes de modificar archivos, usa requirements-clarifier.
Analiza primero, propón un plan y espera mi aprobación.
```

## Flujo esperado

Para cambios no triviales, el agente debería responder primero con un plan similar a:

```text
### Solicitud entendida
...

### Archivos a revisar / modificar
...

### Riesgos
...

### Plan mínimo seguro
...

### Criterios de aceptación
...

### Confirmación requerida
Confírmame si implemento este plan o si ajustamos el alcance primero.
```

Después de revisar el plan, puedes aprobar con:

```text
sí
adelante
confirmo
hazlo
implementa
```

## Estructura del repositorio

```text
coding-agent-skills/
└── requirements-clarifier/
    └── SKILL.md
```

## Notas

Si Claude Code o Codex CLI ya estaban abiertos cuando instalaste la skill, ciérralos y vuelve a abrirlos para asegurar que detecten la nueva skill.

En Codex CLI se recomienda combinar esta skill con permisos restrictivos usando `/permissions` y seleccionando `Read Only`.

Esta skill está pensada para reducir errores por ambigüedad, cambios fuera de alcance, refactors innecesarios y modificaciones sin aprobación previa.
