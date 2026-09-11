# 🎱 Fala Sinuca

O ranking, o ladder e as apostas do grupo, saindo da planilha e indo para um site
com banco de dados de verdade. Multiusuário, nada se perde.

São só 4 arquivos:

| Arquivo | Para que serve |
|---|---|
| `index.html` | O site inteiro, **já com suas chaves do Supabase dentro**. É o único arquivo que vai para o GitHub/Vercel. |
| `1-banco.sql` | Cola no Supabase. Cria tudo e já carrega seus 13 jogadores e 34 partidas. |
| `2-admin.sql` | Cola no Supabase. Libera você como administrador. |
| `README.md` | Este aqui. |

---

## Instalação — 10 minutos

### 1. Supabase

1. Crie a conta em [supabase.com](https://supabase.com) e um projeto novo (o plano grátis basta).
2. Menu **SQL Editor → New query**. Cole o `1-banco.sql` inteiro e clique em **Run**.
   No fim tem de aparecer `Carga concluída: 14 jogadores, 34 partidas.`
3. Menu **Authentication → Users → Add user**. Coloque seu e-mail e uma senha forte,
   e marque **Auto Confirm User**.
4. Abra o `2-admin.sql`, troque o e-mail de exemplo pelo seu, cole no SQL Editor e rode.
   Tem de responder `Admin liberado: Caio`.

As chaves do seu projeto (`qbblrlclvccelpvuecih`) já estão dentro do `index.html`.
Não precisa copiar nada em **Project Settings → API** — só se um dia você trocar de projeto.

A chave `anon` é pública de propósito: sozinha ela não abre nada, porque quem manda são as
regras de segurança do `1-banco.sql`. Pode subir no GitHub sem medo. O que nunca vai para o
repositório é a **senha do seu usuário admin**.

### 2. GitHub

Crie um repositório novo, clique em **Add file → Upload files** e arraste o `index.html`.
Pronto. (Se preferir o terminal: `git init`, `git add .`, `git commit`, `git push`.)

### 3. Vercel

1. Entre em [vercel.com](https://vercel.com) com a conta do GitHub.
2. **Add New → Project** → escolha o repositório → **Deploy**. Não configure nada.
3. Em um minuto sai o link. É esse link que você manda no grupo.

Toda vez que você atualizar o arquivo no GitHub, a Vercel republica sozinha.

---

## Como funciona no dia a dia

**O jogador**

1. Abre o link e clica no próprio nome. Sem senha — o site lembra da escolha no aparelho dele.
2. Vai em **Ladder**. Em quem está até 2 posições acima aparece o botão **Desafiar**.
   Em todo o resto do pessoal aparece **Amistoso**.
3. O jogo marcado aparece para o grupo inteiro apostar moedas.

3. Terminou o jogo, **ele mesmo lança o vencedor** na aba Desafios: aparecem os botões
   *Eu venci* · *Fulano venceu* · *Empate*. Só os dois que jogaram veem esses botões.

**Você**

1. Clica em **Entrar como administrador** e usa o e-mail e a senha do Supabase.
2. Aparece a aba **🔒 ADM**. Em *Lançar resultado*, escolhe quem venceu — útil quando
   ninguém lançou, quando o jogo foi fora do app, ou para corrigir algo.
   É também onde você ajusta os **cinturões** de cada um.
3. Salvou: rating, ladder e ranking se atualizam e **as apostas são pagas na hora**.

Em qualquer um dos dois caminhos fica registrado **quem lançou** — aparece no histórico da
aba Jogos e na auditoria. Se o resultado sair errado, você apaga a partida pela ADM e as moedas voltam.

### Desafio × Amistoso

| | Desafio | Amistoso |
|---|---|---|
| Contra quem | só até 2 posições acima | qualquer um, a qualquer hora |
| Vale rating | ✅ | ✅ |
| Entra no ranking | ✅ | ✅ |
| Muda a posição no ladder | ✅ quem ganha assume o lugar | ❌ ninguém sai do lugar |
| Dá para apostar | ✅ | ✅ |

O amistoso resolve o caso de quem não alcança ninguém: o #13 pode jogar com o #1, valendo
rating e moedas, sem furar a fila do ladder. Quem está em #1 também usa, já que não tem
ninguém acima para desafiar.

Na hora de lançar o resultado, o amistoso **já vem marcado como "vale só rating"** — você
não precisa trocar nada, e mesmo que troque sem querer, o banco não deixa ele mexer no
ladder. Ainda existe uma terceira opção, **"Não conta nada"**, para treino ou jogo em dupla.

---

## Quem pode fazer o quê

| | Jogador | Você |
|---|:--:|:--:|
| Ver ranking, ladder, jogos e saldos | ✅ | ✅ |
| Lançar o próprio desafio ou amistoso | ✅ | ✅ |
| Trocar a própria foto e o próprio emoji | ✅ | ✅ |
| Apostar as próprias moedas | ✅ | ✅ |
| Lançar o vencedor da **própria** partida | ✅ | ✅ |
| Lançar o vencedor de partida dos outros | ❌ | ✅ |
| Ajustar os cinturões | ❌ | ✅ |
| Criar ou tirar moedas | ❌ | ✅ |
| Cadastrar jogador, ajustar posição, apagar jogo | ❌ | ✅ |
| Mudar configurações e ler a auditoria | ❌ | ✅ |

Isso não é só a tela escondendo botão — as regras estão dentro do banco. Testei atacando:
um visitante anônimo tentando inserir jogador, criar 999.999 moedas, mudar o próprio
rating, apagar jogo ou ler a auditoria recebe `permission denied` em todos os casos.

---

## As abas

- **Ranking** — rating, V/E/D, pontos, forma dos últimos 5 jogos e o Fala Score.
  No topo, a faixa dourada do **cinturão atual**. Embaixo, o ranking dos apostadores por lucro.
- **Ladder** — a escada. *Desafiar* em quem está ao alcance, *Amistoso* em todo o resto.
- **Jogos** — duas partes na mesma tela. Em cima, os **jogos marcados**: odds ao vivo,
  botão de apostar, e os botões de lançar o vencedor para quem jogou. Embaixo, o
  **histórico** completo com o Δ rating de cada partida, zebra e troca de posição.
- **Carteira** — saldo, extrato e o Fala Score de apostador.
- **Perfil** — foto, cinturões conquistados, Fala Score detalhado, evolução do rating e
  confrontos diretos (H2H). É aqui que cada um se personaliza.
- **🔒 ADM** — só você. Tem também o botão de **baixar backup em JSON**.

---

## As regras, iguais às da planilha

**Rating** — todo mundo começa em 1000.

| Diferença de rating | Favorito vence | Favorito perde | Zebra vence | Zebra perde |
|---|---:|---:|---:|---:|
| até 50 | +15 | −20 | +20 | −15 |
| 51 a 150 | +20 | −35 | +35 | −30 |
| 151 a 250 | +25 | −50 | +50 | −45 |
| 251 a 350 | +30 | −80 | +80 | −70 |
| acima de 350 | +35 | −100 | +100 | −100 |

**Ladder** — desafia quem está até 2 posições acima. Ganhou, troca de lugar. Perdeu, fica
onde está, mas o rating troca pontos do mesmo jeito. Fora desse alcance, é amistoso: vale
rating e ranking, mas ninguém muda de posição.

**Odds** — `1 + 10^((rating do adversário − seu rating) / 540)`, igual à planilha.
A odd fica **travada no momento em que a pessoa aposta**.

**Ranking** — vitória 3 pontos, empate 1, derrota 0. Ordem por rating.

Rating e posição **não ficam salvos** no banco: são recalculados a partir do histórico
toda vez. Por isso nunca dessincroniza — e corrigir uma partida antiga arruma sozinho
tudo o que veio depois.

---

## Foto e emoji

Cada um personaliza o próprio perfil: **Perfil → Personalizar**. Dá para escolher um emoji
de uma grade (ou digitar qualquer um) e mandar uma **foto**. A foto aparece no lugar do
emoji em tudo — tela de entrada, ranking, ladder, cards de jogo e perfil.

A imagem é **cortada em quadrado e reduzida para 256px dentro do próprio celular** antes de
subir, então fica em uns 20 KB e é guardada direto na tabela de jogadores. Você não precisa
configurar Storage nenhum no Supabase, e não tem custo de armazenamento para se preocupar.

---

## O cinturão

Quem está em **#1 do ladder é o dono do cinturão**. Isso aparece em três lugares:

- Uma **faixa dourada** no topo do Ranking e do Ladder, com o nome do campeão atual.
- A etiqueta **👑 cinturão** e a linha destacada em dourado na primeira posição do ladder.
- No Perfil dele, o selo **👑 CINTURÃO ATUAL**.

Do lado do nome de cada um aparece **🏆 ×N** — quantos cinturões a pessoa já teve na
história do grupo. Esse número é seu: você lança na mão em **ADM → Jogadores → Cinturões**,
e ele entra no ranking, no ladder e no perfil na hora. Ao cadastrar alguém novo dá para
já informar quantos títulos ele tem.

---

## Fala Score (0 a 100)

Um número para resumir o momento de cada um. Fórmula fixa, igual para todos, e visível
dentro do app na aba Perfil.

**Jogador** — rating relativo 40% (850 pts vale 0, 1250 vale 100), aproveitamento 25%,
forma nos últimos 5 jogos 20%, atividade nos últimos 90 dias 15%.
Faixas: Iniciante · Amador · Regular · Bom de Taco · Craque · **Lenda do Fala**.

**Apostador** — ROI 40% (−50% vale 0, 0% vale 50, +50% vale 100), taxa de acerto 25%,
faro 15% (odd média das que acertou, premia zebra), volume 10%, disciplina 10%
(penaliza concentrar tudo numa aposta só).
Faixas: Pé-frio · Apostador · Equilibrado · Faro Bom · Tubarão · **Oráculo**.

Quem tem pouca amostra (menos de 8 partidas ou 10 apostas) tem o score puxado para 50,
para ninguém virar Lenda com uma vitória de sorte.

---

## Sobre os dados

- **A carteira é um livro-caixa que não aceita alteração nem exclusão.** Nem por você,
  nem pelo banco. Correção entra como estorno, e o histórico fica inteiro.
- **Nada fica guardado no navegador.** A única coisa no aparelho da pessoa é qual nome
  ela escolheu, para não ter que clicar de novo. Limpar o navegador não perde nada.
- **Auditoria.** Todo desafio, aposta, resultado, moeda e exclusão fica registrado com
  quem fez e quando.
- **Backup.** Botão na aba ADM. Vale rodar uma vez por mês. O Supabase também faz backup
  automático diário.

### O que o login sem senha não protege

Como você pediu entrada só escolhendo o nome, quem tiver o link pode escolher qualquer
nome e, com isso, apostar as moedas de outro ou lançar um desafio no lugar dele. Para um
grupo de amigos costuma ser tranquilo, e tem três amortecedores:

1. Ninguém consegue **criar** moedas — só você.
2. Toda aposta e todo desafio ficam na auditoria com data e hora.
3. Você pode apagar um jogo pela ADM e as moedas voltam automaticamente.

Se um dia o valor crescer e isso incomodar, dá para acrescentar um PIN de 4 dígitos por
jogador sem refazer nada.

---

## Um detalhe da sua planilha

Ao converter, conferi as 34 partidas uma a uma: **99 de 102 verificações bateram exatamente**
com o que o Excel calculou. As 3 diferenças são células que o Excel deixou desatualizadas
depois de uma edição na coluna VALE. A mais relevante é a **linha 22** (Oscar × Ricardo,
16/07): está marcada como `Sim`, com Oscar em #5 e Ricardo em #7 — desafio válido — mas a
planilha registrou como "só rating" e não trocou as posições.

No Excel dá para conferir apertando **Ctrl+Alt+F9** (recálculo total).

Como isso mexeria no ranking que o grupo já conhece, o `1-banco.sql` **carrega exatamente
o rating e as posições que estão na planilha hoje**. As 34 partidas entram como histórico:
contam para estatística e confronto direto, mas não recalculam nada. Daqui para frente,
todo jogo novo calcula normalmente. Ninguém abre o app e acha que perdeu posição.

Se você preferir o recálculo do zero (matematicamente correto), é só pedir que eu gero
a carga alternativa.
