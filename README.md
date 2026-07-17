# O Tesouro de Benito Soto

Caça ao tesouro digital de página única — o fecho do livro infantil sobre o pirata galego Benito Soto (1805–1831). Site estático (HTML/CSS/JS puro), pensado para telemóvel, ao ar livre, com contraste alto.

## Ficheiros
```
index.html      → o site inteiro (grelha, desafio final, estados, anti-batota)
a1831.mp4        → vídeo do Benito, comprimido (~230 KB, nome não-óbvio de propósito)
benito.png       → retrato a lápis (cabeçalho)
carta1.png       → 1.ª página do manuscrito (só para IMPRIMIR — não é publicada)
carta2.png       → 2.ª página do manuscrito (só para IMPRIMIR — não é publicada)
vercel.json      → cache longa para os media
.vercelignore    → exclui do deploy o vídeo original, a carta e ficheiros de dev
videos/          → vídeo original (NÃO é publicado)
```

## Publicar
O repositório GitHub (`fma77/benito`) está ligado ao Vercel: **cada `git push`
para `main` faz deploy automático**. Não precisa de build — é 100% estático
(Framework Preset: *Other*, sem Build Command nem Output).

## Como funciona (para o guardião)
- **Sem pistas no site** (versão difícil): não há botão de pista nem carta digital. As crianças usam a **carta impressa** (`carta1.png`/`carta2.png`, que ficam no repositório mas **não** são publicadas).
- Grelha 14×14. O número na célula inicial de cada palavra é o **número do parágrafo** da carta impressa (por isso aparecem "fora de ordem" — é intencional).
- Cada palavra certa "assenta" (fixa-se); as 6 células douradas acendem e soletram a palavra-chave.
- Grelha completa → **desafio final**: escrever a palavra das 6 células douradas (1→6), com botão "👁 Espreitar a grelha".
- Acertando → vídeo do Benito (com som) → pergaminho com o botão para enviar o e-mail ao guardião (`benito.soto.aboal.1805@gmail.com`, assunto `Resgate do Tesouro — …`).
- **Sem persistência**: cada refresh recomeça do zero.

## Anti-batota
- As respostas **não** estão no código: só existem os **SHA-256** (com sal) das palavras normalizadas.
- A palavra-chave nunca aparece em claro — é derivada em runtime das células douradas, e o desafio final valida contra elas.
- Nada é guardado no browser (sem `localStorage`) — não há estado para inspecionar nem manipular.
- A carta não é publicada no site — só existe em papel.
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
