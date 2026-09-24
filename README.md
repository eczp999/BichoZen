# BichoZen — V6 (página única)

Site novo da **Clínica Veterinária Bicho Zen**, Londrina/PR, construído do zero
com o modelo da landing page do **HVPET** (`hvpet.com.br/veterinaria-24h-londrina/`)
como referência de estrutura e de fluxo.

**A diferença para o V2:** o V2 tem 25 páginas. Este tem **uma**. Quem chega
desesperado às 3 da manhã não navega menu — ele rola a tela e aperta um botão.

```
index.html      ← o site inteiro (HTML + CSS + JS num arquivo só, 103 KB)
404.html
assets/img/     favicon.svg + apple-touch-icon.png
robots.txt · sitemap.xml · .htaccess · _redirects
```

Sem framework, sem build, sem dependência. Abre com dois cliques, sobe em
qualquer hospedagem.

---

## O que mudou do V5 para o V6

### 1. O modo escuro era o problema — e foi removido

Da V3 à V5 o site tinha um bloco `@media (prefers-color-scheme: dark)`: quando o
sistema ou o navegador do visitante está em tema escuro, ele trocava o creme por
`#131714`. **Era isso que aparecia na tela** — o creme existia no código e nunca
era exibido para quem tem o tema escuro ligado.

No V6 o modo escuro **não existe mais**, em nenhuma linha:

- os dois blocos `@media (prefers-color-scheme: dark)` foram apagados;
- `color-scheme: light` no `:root` e `<meta name="color-scheme" content="light">`
  impedem o navegador de escurecer campos de formulário por conta própria;
- `theme-color` saiu de `#1C5240` (verde escuro) para `#F3EBDC` (o creme).

E o verde escuro saiu de toda área grande:

| Onde era escuro | Como ficou |
|---|---|
| Faixa do topo (verde `#123528`) | Mesmo creme da página, com filete de borda |
| Painéis da seção de exóticos | Cartão branco com filete verde à esquerda |
| Painel do animal silvestre | Cartão vinho claro |
| Bloco de fechamento (degradê verde) | Cartão branco com borda verde no topo |
| Mapa (filtro de inversão) | Sem filtro |

Verificação feita no navegador: uma varredura em todos os elementos maiores que
40.000 px² não encontrou **nenhuma área escura** na página. O verde-floresta
continua na marca, mas agora só em **texto, ícone, borda e botão** — nunca
preenchendo fundo. O único preenchimento escuro que restou é o **botão de
emergência**, em vinho, que é sinalização e foi mantido de propósito.

### 2. As setas do formulário

O bug das setas verdes repetidas também era do modo escuro: lá dentro eu usei o
atalho `background:`, que zera `background-repeat` — a seta voltava a ladrilhar o
campo inteiro. Com o modo escuro fora, o atalho virou `background-color`, entraram
os prefixos `-webkit-`/`-moz-appearance` e `::-ms-expand`, e a seta agora é **uma
só**, cinza discreta, no canto direito.

### 3. Textos mais profissionais

| Antes | Agora |
|---|---|
| "Perguntar se atendemos o meu bicho" | "Consultar o atendimento para o seu animal" |
| "Perguntar se atendemos o meu" | "Consultar atendimento para o meu animal" |
| "Pergunte antes de se deslocar" | "consulte a clínica antes de se deslocar" |
| "Pergunte antes de sair de casa" | "Consulte a recepção antes de sair de casa" |

### 4. Nova seção: "Quem já passou por aqui"

Galeria de pacientes entre a equipe e os depoimentos, com **8 espaços de foto**
(4 colunas no computador, 2 no celular). Os quatro primeiros já vêm rotulados com
casos que a própria clínica publicou: **Zezinho** (filhote de ema), a
**píton-carpete**, o **jabuti** e a **carpa** da coleta de sangue. Os outros
quatro são calopsita, coelho, gato e cão.

Enquanto a foto não existe, o espaço mostra um painel de marca com o desenho da
espécie — nunca banco de imagens (§28.5 do briefing). **Para colocar a foto**,
troque a `div.foto-vazia` por uma `<img>`; o comentário `<!-- SLOT DE FOTO: ... -->`
dentro de cada card diz exatamente qual imagem vai ali:

```html
<div class="foto">
  <img src="assets/img/pacientes/zezinho.webp"
       alt="Zezinho, filhote de ema, atendido na BichoZen" loading="lazy">
</div>
```

⚠️ Fotos de paciente só entram com **autorização do tutor**.

### 5. Equipe com foto e três vagas

Pesquisei nomes de outros profissionais em fonte pública e **não encontrei**: o
domínio antigo (`clinicabichozen.com.br`, que tinha uma página `/veterinarios/`)
segue fora do ar — NXDOMAIN —, e Instagram e Facebook exigem login. Um site que
aparecia como "avaliações da clínica" virou página de spam de cassino.

Então a seção ficou assim:

- **Sócios-fundadores** — Dra. Luísa e Dr. Rafael, agora com **espaço de foto**
  em cada card, além do texto que já existia;
- **Equipe de plantão** — **três cards vazios**, com espaço de foto, "Nome a
  confirmar", "CRMV-PR a confirmar" e a função. É só preencher.

### 6. Depoimentos apresentáveis

Entrou um bloco de nota com **4,5** e as estrelas desenhadas na proporção exata
(4,5 de 5 = 90% preenchido), com link para as avaliações no Google. A nota foi
**conferida no Perfil da Empresa no Google em 23/09/2026**.

Os três depoimentos continuam os mesmos — são trechos reais — mas ganharam ícone
de aspas, etiqueta de assunto, linha de autoria com avatar e a fonte. O nome de
cada tutor está como **"Nome do tutor a confirmar"**: não dá para inventar nome de
pessoa, e o briefing (§19) proíbe depoimento anônimo fabricado.

⚠️ **Dois pontos para decidir antes de publicar:**

1. **O número de avaliações não foi confirmado.** O Google Maps mostra a nota mas
   não o total sem login; agregadores dizem "77+" e o briefing registrou 81. O
   site marca isso como pendência em vez de chutar.
2. O briefing **§28.12 recomendava não exibir a nota agregada** enquanto o volume
   for baixo — 4,5 é a menor nota entre os serviços 24h da cidade. A nota está no
   site porque foi pedida; se preferir tirar, é só apagar o bloco `.nota`.

**O caminho que resolve os dois:** pedir avaliação a quem sai satisfeito. Três
avaliações com nome, cidade e espécie do pet valem mais que qualquer ajuste de
layout — e sobem a nota.

---

## O que mudou do V4 para o V5

### 1. A cor, agora de verdade

O V4 saiu de `#FBF8F3` para `#FDFAF1` — no papel é mais creme, na tela ninguém
via diferença. O V5 usa **`#F3EBDC`**, um creme de papel que se lê à primeira
vista, e continua valendo a regra do V4: **o mesmo tom no site inteiro**, do topo
do hero ao rodapé, sem nenhuma seção com fundo próprio.

| | Fundo | Cartões | Borda |
|---|---|---|---|
| V3 | `#FBF8F3` + faixas bege e verde | `#FFFFFF` | `#E9E2D6` |
| V4 | `#FDFAF1` (uniforme) | `#FFFFFF` | `#EBE3D1` |
| **V5** | **`#F3EBDC`** (uniforme) | `#FFFFFF` | `#DFD4BC` |

O relevo vem do contraste entre o creme da página e o **branco dos cartões** — é
isso que dá profundidade sem precisar cortar o site em faixas de cor. Também saiu
o degradê que o hero tinha no canto superior direito: era mais um ponto em que o
tom mudava.

Contraste conferido sobre o creme novo: texto 14,9:1 · verde da marca 7,9:1 ·
vinho 8,2:1 · texto secundário 6,7:1. Tudo acima do AA, a maior parte em AAA.

### 2. Ficha de agendamento no fim do site

Nova seção `#agendar`, entre "Como chegar" e o fechamento — inspirada na da
Clínica PIO, com os campos que uma clínica veterinária precisa:

| Campo | |
|---|---|
| Nome completo | obrigatório |
| Telefone / WhatsApp | obrigatório |
| E-mail | opcional |
| Nome do animal | opcional |
| **Espécie** | obrigatório — 11 opções, de cão e gato a serpente, jabuti, roedor, peixe e silvestre |
| Tipo de atendimento | 9 opções (primeira consulta, rotina, retorno, vacinação, castração, odontologia, exótico, caso crônico, "ainda não sei") |
| Melhor horário | manhã, tarde, noite, madrugada, qualquer |
| Profissional | sem preferência, Dra. Luísa (felinos), Dr. Rafael (exóticos) |
| **O que você notou** | campo livre para os sintomas — o que mudou, há quanto tempo |

**Como o envio funciona.** O site é estático, não tem servidor. Ao enviar, a ficha
vira uma **mensagem pronta no WhatsApp da clínica** — o mesmo mecanismo do site da
Clínica PIO. O tutor confere o texto e manda. É o canal que a recepção já opera
todo dia, então nenhuma mensagem cai num e-mail que ninguém abre.

A mensagem que chega na recepção sai assim:

```
Olá! Vim pelo site e gostaria de agendar uma consulta.

Tutor: Enrico Pessoa
Telefone: (43) 99999-1234
E-mail: enrico@exemplo.com
Animal: Zeca (Jabuti ou tartaruga)
Atendimento: Avaliação de pet exótico
Melhor horário: Tarde
Profissional: Dr. Rafael Saconatto — pets exóticos

O que notei: Parou de comer há 3 dias e está com o olho inchado.
```

Detalhes que fazem diferença na recepção:

- **Emergência não passa pelo formulário.** Ao lado da ficha há um aviso em vinho
  com o botão de ligar. Quem está com o bicho passando mal não preenche campo.
- **Validação sem alerta chato:** os três campos obrigatórios ficam com borda
  vermelha e mensagem embaixo; o foco pula para o primeiro que faltou.
- **Se o pop-up for bloqueado**, o próprio navegador é redirecionado para o
  WhatsApp em vez de não acontecer nada.
- **Sem JavaScript**, aparece um aviso com o WhatsApp e o telefone da clínica.
- A seção diz, em letra miúda, que **nada fica guardado no site** — o que é
  verdade e evita promessa de agendamento automático que a clínica não tem.

⚠️ **Confirmar com a clínica antes de publicar:** o número (43) 99142-1179 vai
receber as fichas. Alguém responde? Em que horário? É o mesmo número do curso? O
briefing (§24) alerta para o risco de sobrecarregar um número que não está
preparado — e uma ficha sem resposta é pior que nenhuma ficha.

### 3. Navegação

Entrou **"Agendar"** no menu do topo (e no rodapé). "Pets exóticos" virou
"Exóticos" para o menu caber sem apertar. Na triagem, o botão secundário da porta
"quero agendar para meu cão ou gato" agora leva à ficha em vez de repetir a lista
de serviços.

---

## O que mudou do V3 para o V4

**1. Um único tom de fundo no site inteiro.**
O V3 alternava creme → bege → creme → verde-escuro. Agora o fundo é **um só**
(`--bg: #FDFAF1`, creme mais claro e mais quente), do topo do hero até o rodapé.
Nenhuma seção tem cor de fundo própria — a hierarquia vem dos **cartões brancos**,
das bordas e do espaçamento.

O que continua colorido, de propósito, são **elementos** e não fundos de seção:
a faixa de status no topo, os dois painéis verdes da seção de exóticos, o cartão
vinho do animal silvestre e o cartão verde do CTA final (que no V3 era uma faixa
de ponta a ponta e agora é um cartão arredondado dentro do creme).

**2. Card "Cuidado canino" nos serviços.**
Tinha "Medicina felina" e não tinha o equivalente para cães — quem procura só isso
não se via na lista. Entrou como o 3º card, ao lado do de gatos. Para o grid não
ficar com uma sobra, o card "Curso para veterinários" saiu da grade (ele continua
na seção de exóticos, com botão próprio) — a grade seguiu com 9 cards, 3×3.

**3. Ícones novos — todos.**
As ilustrações autorais do V3 saíram. O site agora usa **Lucide** (`lucide.dev`,
licença ISC, © Lucide Contributors), um conjunto profissional de traço único.
Estão embutidos no próprio HTML, como `<symbol>` — nenhuma requisição extra.

O Lucide não tem serpente nem lagarto, então esses dois foram desenhados no mesmo
grid (24×24, traço 2, cantos arredondados) para não destoar. O dente da
odontologia também é próprio, pelo mesmo motivo.

Como agora o estilo vem do CSS (`svg{fill:none;stroke:currentColor;...}`) e não de
atributo em cada ícone, **trocar um ícone é colar o `<path>` novo dentro do
`<symbol>` correspondente** — o tamanho e a cor continuam funcionando sozinhos.

Sobram 4 símbolos definidos e não usados (`i-book`, `i-heart`, `i-syringe`,
`i-agenda`): são reserva para quando entrar um card novo.

**4. Botão de emergência.**
Virou o elemento mais visível da página: 58px de altura, degradê vinho, sombra
colorida, bolinha pulsando e um anel que pulsa em volta (`.btn--emerg .btn--pulse`).
No celular ocupa a largura toda. O anel some em `prefers-reduced-motion`.

**5. Celular em primeiro lugar.**
Sem rolagem lateral em 375px, alvos de toque de 48px+ (o de emergência, 58px),
barra fixa embaixo com Ligar / WhatsApp / Rota, e a tabela comparativa cabendo
inteira na tela — que é onde ela mais convence.

---

## O que foi copiado do modelo (HVPET) (HVPET) — e o que foi adaptado

| Do modelo | Como ficou aqui |
|---|---|
| Barra de topo com "aberto agora" + telefone | Mantida, com bolinha pulsando |
| Cabeçalho fixo com Ligar + WhatsApp | Mantido |
| Hero com selo, título, CTAs e 3 números | Mantido — **sem foto**: no lugar entra o quadro de espécies |
| Grade de serviços com selo "24h" | Mantida (9 cards) |
| **Tabela comparativa** | Mantida — é a parte que você mais gostou |
| Equipe com credenciais | Mantida (os 2 sócios) |
| Depoimentos | Mantidos, com as avaliações reais do Google |
| Tabela de distâncias por cidade | **Cortada** — a clínica não confirmou de onde vêm os tutores |
| FAQ sanfona | Mantido (6 perguntas) |
| Localização + mapa + CTA final + rodapé | Mantidos |
| WhatsApp flutuante | Mantido + barra fixa no rodapé do celular |

**O que o modelo não tinha e entrou aqui:** a *triagem em três portas* logo abaixo
do hero (emergência × pet exótico × cão ou gato) e a seção de exóticos. É onde
está a vantagem real da BichoZen sobre o HVPET.

### A tabela comparativa

O HVPET compara "hospital 24h × clínica de plantão". Aqui a comparação é outra,
porque a vantagem da BichoZen é outra: **"clínica veterinária comum × BichoZen"**,
com 9 linhas — aves, répteis, coelhos, peixes, plantão real, internação de
exótico, medicina felina dedicada e o veterinário que dá curso na área.

A primeira linha (cão e gato) é ✓ nos dois lados **de propósito**: tabela em que
o concorrente perde em tudo não convence ninguém.

Abaixo da tabela há uma nota dizendo que o comparativo é genérico e não se refere
a nenhuma clínica de Londrina. **Não tire essa nota** — ela é o que separa
comparação honesta de propaganda enganosa.

---

## O que continua valendo do briefing

- **Zero foto de banco de imagens.** As ilustrações de espécies são autorais
  (SVG, traço 2px), as mesmas do V2.
- **Nada de serviço inventado.** Só o que está confirmado. A seção de serviços
  termina pedindo que o tutor pergunte antes de se deslocar.
- **Sem a nota 4,5** do Google (volume baixo). Os 3 depoimentos são trechos reais,
  atribuídos a "Avaliação pública no Google", sem nome inventado.
- **Paleta verde-floresta + vinho**, fora do azul-clínico. Vinho só para urgência.
- Modo escuro, contraste AA/AAA, alvos de toque ≥ 44px, foco visível,
  `prefers-reduced-motion` respeitado, conteúdo aparece mesmo sem JS.

---

## Pendências (as mesmas do V2 — o site novo não as resolve)

1. **Domínio** `clinicabichozen.com.br` estava fora do ar. Verificar no Registro.br.
2. **CRMV-PR do Responsável Técnico** — exigência legal. Aparece no rodapé como
   selo tracejado `CRMV-PR a confirmar`. Idem o CRMV individual dos dois sócios.
3. **Lista oficial dos plantonistas.**
4. **Fotos.** O site funciona sem nenhuma, mas melhora muito com elas.

Antes de publicar, os selos tracejados podem ser escondidos acrescentando
`.pend{display:none}` ao fim do `<style>` — **menos o do CRMV do RT**, que precisa
ser preenchido, não escondido.

---

## Como mexer

Está tudo em `index.html`, em ordem de leitura, com comentários marcando cada
seção (`<!-- ===== SERVIÇOS ===== -->` etc.).

- **Cor:** no começo do `<style>`, bloco `:root`.
- **Telefone:** procure `3037-0755` (aparece em ~8 lugares, todos visíveis).
- **WhatsApp:** os links já vão com mensagem pronta e diferente por seção
  (emergência, exótico, agendar, Dra. Luísa, Dr. Rafael, curso). Serve de
  analytics: a recepção sabe de onde veio o contato.
- **Texto:** é HTML puro, dá para editar no bloco de notas.

### Publicar

Subir os arquivos para a hospedagem. `.htaccess` (Apache) e `_redirects`
(Netlify/Vercel) já trazem os 301 das URLs antigas (`/a-clinica/`, `/servicos/`,
`/veterinarios/`, `/contato/`) apontando para as âncoras da página nova.

### Ver no navegador

```bash
python -m http.server 4183
```

E abrir `http://localhost:4183/`.
