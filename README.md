# Planejamento Estratégico Goiás

Ferramenta colaborativa para o planejamento estratégico por grupos de trabalho:
Problemas → Ishikawa (6M) → Matriz GUT com votos → Plano 5W2H → exportação para Excel.

- Página estática publicada no **GitHub Pages**.
- Banco em tempo real no **Firebase Firestore** (plano gratuito Spark).
- Ninguém precisa criar conta: cada pessoa digita o nome e escolhe o grupo.
  O navegador guarda essa identificação (login anônimo do Firebase).

## Arquivos

| Arquivo | Para que serve |
|---|---|
| `index.html` | O sistema inteiro |
| `firebase-config.js` | Os dados do seu projeto Firebase (você preenche) |
| `firestore.rules` | As regras de segurança do banco (você cola no Firebase) |

---

## Parte 1 · Firebase (uns 10 minutos)

1. Acesse https://console.firebase.google.com com uma conta Google e clique em **Criar projeto**.
   Nome sugerido: `planejamento-goias`. O Google Analytics pode ficar desativado.
2. **Banco de dados:** menu **Build › Firestore Database › Criar banco de dados**.
   - Local: `southamerica-east1 (São Paulo)`.
   - Modo: **produção**.
3. **Regras:** na aba **Regras** do Firestore, apague tudo, cole o conteúdo do arquivo
   `firestore.rules` e clique em **Publicar**.
4. **Login anônimo:** menu **Build › Authentication › Vamos começar › Método de login**,
   escolha **Anônimo**, ative e salve.
5. **Registrar o app:** ⚙️ **Configurações do projeto › Seus apps**, clique no ícone **`</>`** (Web),
   dê um apelido (ex.: `site`) e **não** marque Firebase Hosting.
   Copie os valores de `firebaseConfig` que aparecem e cole no arquivo `firebase-config.js`,
   substituindo os valores de exemplo.

## Parte 2 · GitHub Pages (uns 5 minutos)

1. Em https://github.com/new crie um repositório **público** (ex.: `planejamento-goias`).
2. Na página do repositório, clique em **Add file › Upload files**, arraste os 4 arquivos
   (`index.html`, `firebase-config.js`, `firestore.rules`, `README.md`) e clique em **Commit changes**.
3. Vá em **Settings › Pages**. Em *Build and deployment*, escolha **Deploy from a branch**,
   branch **main**, pasta **/(root)** e clique em **Save**.
4. Em 1 a 2 minutos o endereço aparece no topo da página, no formato
   `https://SEU-USUARIO.github.io/planejamento-goias/`.

## Parte 3 · Primeiro acesso (antes de divulgar o link)

1. Abra o endereço do site, digite **seu nome** e clique em **Configurar como organizador**.
   Essa opção só aparece enquanto ninguém configurou; depois ela some para sempre.
2. Em **Grupos de trabalho**, cadastre os grupos: PERDAS BT, PERDAS AT, MEDIÇÃO FISCAL, GAMBIARRA.
3. Para ter outros coordenadores: depois que a pessoa abrir o link e informar o nome,
   ela aparece em **Grupos de trabalho › Coordenadores**. Clique em **Tornar coordenador**.
4. Faça um teste com 2 colegas. Depois é só excluir os itens de teste.

Agora é só divulgar o link (Teams, e-mail, WhatsApp ou QR code no telão).

---

## Como funciona o acesso

- Cada navegador recebe uma identidade anônima do Firebase. A pessoa digita o nome uma vez e escolhe o grupo.
- Se a pessoa trocar de navegador ou de aparelho, ela entra como uma pessoa nova
  (digita o nome e escolhe o grupo de novo). Os votos anteriores ficam no outro aparelho.
- Os participantes veem e editam só o trabalho do próprio grupo.
- Coordenadores veem todos os grupos, cadastram e editam grupos e exportam o plano consolidado.

## Segurança: o que saber

- Os dados de `firebase-config.js` **não são senha**. O Firebase foi feito para eles ficarem
  visíveis no código. Quem protege o banco são as regras do `firestore.rules`.
- **Qualquer pessoa com o link consegue participar.** Divulgue só para os participantes.
- Só coordenadores criam ou alteram grupos e coordenadores. Cada pessoa só altera o próprio voto.
- Os dados ficam no seu projeto Firebase, numa conta Google, fora do ambiente da Equatorial.
  Alinhe com a TI ou a liderança se o conteúdo for sensível.

## Depois do evento

1. Entre como coordenador, escolha **Visualizar: Todos os grupos** e clique em **Exportar plano (.xlsx)**.
2. Para congelar o banco (só leitura), troque as regras do Firestore por:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /{document=**} { allow read: if request.auth != null; allow write: if false; }
     }
   }
   ```

## Limites do plano gratuito

O plano gratuito (Spark) permite 50 mil leituras e 20 mil gravações por dia.
No Firestore, cada alteração é "lida" por todas as pessoas com a página aberta:
com 40 participantes, um voto gera cerca de 40 leituras. Num evento com muita votação
na Matriz GUT, dá para chegar perto do limite diário. Se ele estourar, o banco para de
responder até a meia-noite (horário do Pacífico).

Para o dia do evento, a recomendação é ativar o plano **Blaze** (pago por uso) em
**Configurações do projeto › Uso e faturamento**. Ele exige cartão, mas mantém a mesma cota
gratuita e só cobra o excedente, na casa de centavos de dólar por 100 mil leituras.
Dá para criar um alerta de orçamento (ex.: US$ 5) no mesmo lugar.
