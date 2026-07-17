# O Tesouro de Benito Soto

Caça ao tesouro digital de página única — o fecho do livro infantil sobre o pirata galego Benito Soto (1805–1831). Site estático (HTML/CSS/JS puro), pensado para telemóvel, ao ar livre, com contraste alto.

## Ficheiros
```
index.html      → o site inteiro (grelha, pistas, estados, anti-batota)
a1831.mp4        → vídeo do Benito, comprimido (~230 KB, nome não-óbvio de propósito)
benito.png       → retrato a lápis (ABERTURA)      ⚠ substituir o placeholder
carta1.png       → 1.ª página do manuscrito         ⚠ substituir o placeholder
carta2.png       → 2.ª página do manuscrito         ⚠ substituir o placeholder
vercel.json      → cache longa para os media
.vercelignore    → não envia o vídeo original (13 MB) nem ficheiros de dev
videos/          → vídeo original (NÃO é publicado)
```

## ⚠ Antes de publicar: substituir as 3 imagens
`benito.png`, `carta1.png` e `carta2.png` são **placeholders** (blocos de cor).
Copie por cima os ficheiros reais, **mantendo exactamente estes nomes**.

## Publicar no Vercel
Não precisa de build — é 100% estático.

**Opção A — arrastar a pasta**
1. https://vercel.com → *Add New… → Project → Deploy* (ou arrastar a pasta em *vercel.com/new*).
2. Framework Preset: **Other**. Build Command: (vazio). Output: (vazio).

**Opção B — CLI**
```
npm i -g vercel
vercel        # pré-visualização
vercel --prod # produção
```

## Como funciona (para o guardião)
- **Sem pistas no site** (versão difícil): não há botão de pista nem carta digital. As crianças usam a **carta impressa** (`carta1.png`/`carta2.png`, que ficam no repositório mas **não** são publicadas).
- Grelha 14×14. O número na célula inicial de cada palavra é o **número do parágrafo** da carta impressa (por isso aparecem "fora de ordem" — é intencional).
- Cada palavra certa "assenta" (fixa-se); as 6 células douradas acendem e soletram a palavra-chave.
- Grelha completa → **desafio final**: escrever a palavra das 6 células douradas (1→6), com botão "👁 Espreitar a grelha".
- Acertando → vídeo do Benito (com som) → pergaminho com o botão para enviar o e-mail ao guardião (`benito.soto.aboal.1805@gmail.com`, assunto `Resgate do Tesouro — …`).
- **Sem persistência**: cada refresh recomeça do zero.

## Anti-batota
- As respostas **não** estão no código: só existem os **SHA-256** (com sal) das palavras normalizadas.
- A palavra-chave nunca aparece em claro — é derivada em runtime das células douradas.
- `localStorage` guarda apenas *flags* (índices) e as letras já resolvidas ofuscadas — nunca respostas por resolver.
- Normalização: maiúsculas, sem acentos e `Ñ→N` (MARTIÑO valida como MARTINO).
- O vídeo tem um nome não-óbvio (`a1831.mp4`).

## Re-comprimir o vídeo (se trocar o original)
```
ffmpeg -i videos/benito_video.mp4 -vcodec libx264 -crf 27 -preset slow \
  -vf scale=720:-2 -acodec aac -b:a 96k -movflags +faststart a1831.mp4
```

## Testar localmente
```
npx serve .
# abrir http://localhost:3000
```
Nota: o `crypto.subtle` (validação) exige contexto seguro — use `localhost` ou `https`, não abra o ficheiro por `file://`.
