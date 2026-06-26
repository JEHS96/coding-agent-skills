# Claude Skills

Colección de skills para Claude Code orientadas a flujos de desarrollo más seguros.

## Skills disponibles

### requirements-clarifier

Skill para evitar cambios prematuros o mal entendidos en el código.

Antes de modificar archivos, obliga a Claude Code a:

- Analizar la solicitud.
- Clasificar el nivel de riesgo.
- Revisar archivos relevantes cuando sea necesario.
- Proponer un plan mínimo seguro.
- Definir criterios de aceptación.
- Pedir aprobación antes de modificar archivos.

La skill permite cambios triviales directos solo cuando son pequeños, reversibles y claramente especificados.

## Instalación en Windows PowerShell

Ejecuta este comando:

```powershell
$skillDir = "$HOME\.claude\skills\requirements-clarifier"
New-Item -ItemType Directory -Force -Path $skillDir | Out-Null

Invoke-WebRequest `
  -Uri "https://raw.githubusercontent.com/JEHS96/claude-skills/main/requirements-clarifier/SKILL.md" `
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

## Instalación en macOS/Linux

Ejecuta:

```bash
mkdir -p ~/.claude/skills/requirements-clarifier

curl -fsSL \
  https://raw.githubusercontent.com/JEHS96/claude-skills/main/requirements-clarifier/SKILL.md \
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

## Instalación clonando el repositorio

También puedes instalar la skill clonando este repositorio:

```bash
git clone https://github.com/JEHS96/claude-skills.git
mkdir -p ~/.claude/skills
cp -R claude-skills/requirements-clarifier ~/.claude/skills/requirements-clarifier
```

En Windows PowerShell:

```powershell
git clone https://github.com/JEHS96/claude-skills.git
New-Item -ItemType Directory -Force -Path "$HOME\.claude\skills" | Out-Null
Copy-Item -Recurse ".\claude-skills\requirements-clarifier" "$HOME\.claude\skills\requirements-clarifier" -Force
```

## Actualizar la skill

### Windows PowerShell

```powershell
Invoke-WebRequest `
  -Uri "https://raw.githubusercontent.com/JEHS96/claude-skills/main/requirements-clarifier/SKILL.md" `
  -OutFile "$HOME\.claude\skills\requirements-clarifier\SKILL.md"
```

### macOS/Linux

```bash
curl -fsSL \
  https://raw.githubusercontent.com/JEHS96/claude-skills/main/requirements-clarifier/SKILL.md \
  -o ~/.claude/skills/requirements-clarifier/SKILL.md
```

## Uso recomendado

Puedes invocarla directamente dentro de Claude Code:

```text
/requirements-clarifier
```

También se activa automáticamente ante solicitudes como:

```text
Implementa este cambio
Corrige este bug
Refactoriza este módulo
Agrega esta validación
Cambia esta regla de negocio
Integra este endpoint
```

## Flujo esperado

Para cambios no triviales, Claude Code debería responder primero con un plan similar a:

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
claude-skills/
└── requirements-clarifier/
    └── SKILL.md
```

## Notas

Si Claude Code ya estaba abierto cuando instalaste la skill, ciérralo y vuelve a abrirlo para asegurar que detecte la nueva skill.

Esta skill está pensada para reducir errores por ambigüedad, cambios fuera de alcance, refactors innecesarios y modificaciones sin aprobación previa.
