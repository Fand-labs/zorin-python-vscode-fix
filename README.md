# Python no Zorin OS 18.1 + VS Code
## Solução para erro GLib-GIO antialiasing

Este guia documenta uma solução testada no Zorin OS 18.1 para executar aplicativos Python pelo VS Code usando Code Runner.

## 1. Ambiente testado

- Zorin OS 18.1
- GNOME 46.1
- Wayland
- Python 3.12.3
- GLib 2.80
- Visual Studio Code
- Extensão Code Runner

## 2. Erro encontrado

```text
GLib-GIO-ERROR:
Settings schema 'org.gnome.settings-daemon.plugins.xsettings'
does not contain a key named 'antialiasing'

Trace/breakpoint trap
```

## 3. Instalar Python

```bash
sudo apt update
sudo apt install python3 python3-pip python3-venv python-is-python3 python3-tk -y
```

Verificar:

```bash
python --version
python3 --version
```

## 4. Testar a solução X11

Na pasta do projeto:

```bash
cd ~/Projetos/Info
GDK_BACKEND=x11 /usr/bin/python3 App.py
```

No ambiente testado, o aplicativo abriu normalmente.

## 5. Configurar VS Code globalmente

Criar a pasta de configurações:

```bash
mkdir -p ~/.config/Code/User
```

Criar o arquivo settings.json:

```bash
cat > ~/.config/Code/User/settings.json <<'JSON'
{
    "code-runner.executorMap": {
        "python": "GDK_BACKEND=x11 /usr/bin/python3 -u"
    },
    "code-runner.runInTerminal": true,
    "code-runner.saveFileBeforeRun": true,
    "python.defaultInterpreterPath": "/usr/bin/python3",
    "python.terminal.activateEnvironment": true,
    "terminal.integrated.env.linux": {
        "GDK_BACKEND": "x11"
    }
}
JSON
```

## 6. Reiniciar VS Code

```bash
pkill -f code
```

Abrir novamente:

```bash
code .
```

Abrir o arquivo Python e testar o botão ▶ do Code Runner.

## 7. Resultado

- Python 3 reconhecido pelo sistema.
- Code Runner executando com X11.
- Terminal integrado configurado.
- Configuração global do VS Code.
- Configurações persistentes após reiniciar o computador.

## 8. Observações

Esta solução foi testada no ambiente descrito acima.

GDK_BACKEND=x11 é uma alternativa de compatibilidade para aplicativos gráficos que apresentam problemas com Wayland.

Não representa uma correção universal do GNOME ou GLib.

Para executar aplicativos Python por duplo clique fora do VS Code, ainda é necessário configurar permissões e arquivos .desktop.

## Compartilhe

Se este guia ajudou você, compartilhe com outros desenvolvedores Linux!

Contribuições e relatos de outros ambientes são bem-vindos.
