# agents.md - MoonlightGlow for Rider

Arquivo de regras para qualquer agente (humano ou IA) que mantenha este plugin.
Idioma de trabalho: portugues. Sem emojis. Sem simbolos decorativos.

## 1. Papel do agente

Voce mantem um plugin de tema do JetBrains Rider 2026.2 (builds 262.*).
Plugin id: com.everfranca.moonlightglow. Versao atual: 1.0.4.
O plugin entrega dois artefatos com o mesmo nome MoonlightGlow:
um UI theme (theme.json, targetUi new) e um editor color scheme (.icls/.xml).
Nunca presuma caminhos ou formatos. Sempre confirme nos arquivos antes de agir.

## 2. Contexto obrigatorio antes de qualquer mudanca

Ordem de leitura, sem excecao:

1. src/main/resources/META-INF/plugin.xml (id, version, themeProvider, bundledColorScheme)
2. src/main/resources/MoonlightGlow.theme.json (cores do chrome)
3. src/main/resources/colorSchemes/MoonlightGlow.xml (cores do editor)
4. CHANGELOG ou historico de versoes, se existir, para nao repetir erro ja corrigido
5. Em caso de debug no Rider instalado: idea.log em %APPDATA%/JetBrains/Rider2026.2 e
   %LOCALAPPDATA%/JetBrains/Rider2026.2/log/idea.log

Nao proponha solucao antes de concluir essa leitura. Evidencia antes de sintese.

## 3. Regras obrigatorias

1. Nao faca nenhuma alteracao sem pedido explicito do mantenedor.
   Diagnostico e plano primeiro, execucao depois.
2. Caminhos do Rider 2026.2 no Windows:
   schemes em %APPDATA%/JetBrains/Rider2026.2/colors/
   themes em %APPDATA%/JetBrains/Rider2026.2/themes/
   Nao existe subpasta config/ dentro dessas pastas. Nunca crie.
3. UI theme so funciona via plugin (extensao themeProvider com targetUi new).
   Arquivo .theme.json avulso copiado para pastas de configuracao e ignorado
   silenciosamente. Nao perca tempo com esse caminho.
4. parentTheme de temas dark new-UI e ExperimentalDark.
   editorScheme do tema e MoonlightGlow.
5. Scheme: raiz <scheme> com name, version e parent_scheme Darcula.
   Em <colors> use somente chaves de cor validas do IntelliJ
   (ex: CARET_COLOR, SELECTION_BACKGROUND, GUTTER_BACKGROUND).
   Atributos de destaque (match brace, breakpoint, erros) vao em <attributes>,
   nunca em <colors>.
6. Toda mudanca visual exige: parse de validacao do XML e do JSON,
   rebuild do .jar a partir de src/main/resources, reinstall via
   Install Plugin from Disk e Restart do Rider antes de declarar pronto.
7. Mudanca experimental exige backup nomeado (ex: backup-v1.0.4/) e bump de
   versao no plugin.xml. Nunca reutilize o numero da versao estavel.
8. NUNCA versione ids pessoais, senhas, tokens, chaves de API ou qualquer
   informacao sensivel: nem em codigo, nem em docs, nem em logs, nem em
   screenshots, nem em nomes de arquivo. Tokens de publicacao do Marketplace
   ficam somente em variavel de ambiente local, fora do repositorio.
9. Screenshots para o Marketplace nao podem conter dados reais de usuario,
   caminhos locais, e-mails ou conteudo proprietario. Use codigo de exemplo.
10. Nao use marca registrada de terceiros no titulo do plugin. Cite
    inspiracao no Visual Studio somente na descricao.

## 4. Boas praticas com exemplos

### 4.1 Ler antes de concluir

Fazer: abrir plugin.xml e confirmar o themeProvider antes de afirmar
por que um tema nao lista.

Nao fazer: adivinhar o caminho de instalacao de cor.

### 4.2 Validar antes de entregar

Fazer: rodar parse do XML e do JSON e so entao rebuildar o .jar.

```
python3 -c "import json, xml.etree.ElementTree as ET; json.load(open('src/main/resources/MoonlightGlow.theme.json')); ET.parse('src/main/resources/colorSchemes/MoonlightGlow.xml'); print('OK')"
```

Nao fazer: entregar .jar montado de arquivos nao validados.

### 4.3 Uma mudanca por versao testavel

Fazer: v1.0.4 alterou apenas toolbars para 0A1E32, com backup da v1.0.3,
permitindo reversao em um passo.

Nao fazer: misturar paleta, fonte e layout na mesma versao sem como
reverter cada parte.

### 4.4 Manter Theme e Scheme com o mesmo nome

Fazer: Theme MoonlightGlow referencia editorScheme MoonlightGlow, e o
plugin registra bundledColorScheme MoonlightGlow. Um nome so na UI.

Nao fazer: deixar o usuario com TestGlow no editor e MoonlightGlow no
chrome sem documentar a divergencia.

## 5. Mas praticas conhecidas neste projeto

Cada item abaixo ja aconteceu e custou uma iteracao. Nao repetir.

### 5.1 Chave invalida no scheme

Errado: colocar EDITOR_LIGATURES como option do scheme, ou
BREAKPOINT_ATTRIBUTES dentro de <colors>. O Rider ignora o arquivo
silenciosamente e o scheme nao lista.

Correto: opcoes de fonte validas (EDITOR_FONT_NAME, EDITOR_FONT_SIZE,
EDITOR_LINE_SPACING) e atributos de destaque somente em <attributes>.

### 5.2 Extensao e local errados do tema

Errado: arquivo MoonlightGlow.jtheme.json copiado para
Rider2026.2/config/themes/. Resultado: nada aparece, nenhum erro no log.

Correto: MoonlightGlow.theme.json empacotado no .jar com extensao
themeProvider no plugin.xml, instalado via Install Plugin from Disk.

### 5.3 Preto puro nas Islands

Errado: Gray0 com valor 000000. Os vaos entre as Islands e superficies
nao sobrescritas exibem preto puro, quebrando o navy.

Correto: rampa de cinzas em tons navy escuros (ex: 060F1D a 0D2340),
nenhum preto puro no tema.

### 5.4 Toolbar herdada do cinza

Errado: confiar que MainToolbar.background herda o navy. A chave resolve
para Gray2 (24272B, quase preto) e a toolbar principal fica preta.

Correto: sobrescrever explicitamente MainToolbar.background,
MainToolbar.Icon.background, ToolWindow.Header e Stripe com o navy
desejado.

### 5.5 Script PowerShell bloqueado

Errado: distribuir somente install-windows.ps1. A ExecutionPolicy padrao
bloqueia script nao assinado e o usuario nao consegue instalar.

Correto: oferecer install-windows.bat como caminho principal no Windows
e documentar a alternativa com ExecutionPolicy Bypass para o .ps1.

## 6. Workflow de mudanca visual

1. Backup da versao estavel em backup-vX.Y.Z/.
2. Editar somente src/main/resources/.
3. Validar JSON e XML por parse.
4. Bump de versao no plugin.xml e registro no README (Version history).
5. Rebuild do .jar a partir de src/main/resources.
6. Install Plugin from Disk, Restart, selecionar Theme e Scheme.
7. Checklist visual: toolbar principal, Solution, status bar, abas,
   editor C# (keyword, string, comment, tipos, metodos, locais),
   selecao, caret row, gutter.
8. Se aprovado, a versao vira a estavel. Se rejeitado, reinstalar o
   backup e registrar o aprendizado na secao 5.

## 7. Publicacao no Marketplace

1. plugin.xml com id unico, vendor, description e change notes.
2. META-INF/pluginIcon.svg 40x40 presente no .jar.
3. LICENSE na raiz do repositorio.
4. Screenshots sem dados sensiveis (VS lado a lado com Rider).
5. Upload manual em plugins.jetbrains.com ou via token em CI.
   Token nunca entra no repositorio (ver regra 8).
