📻 Morse Trainer — Arduino UNO
Um treinador de código Morse com 4 modos, display LCD 16×2 I²C, um botão e buzzer. Ideal para aprender pontos (.), traços (-), letras e números! 🚀
🧰 Componentes
- 1 × Arduino UNO
- 1 × LCD 16×2 com módulo I²C (4 pinos)
- 1 × botão de pressão (push button)
- 1 × buzzer passivo (compatível com tone())
- Protoboard e jumpers
- Cabo USB para alimentar e programar o Arduino
🔌 Ligações elétricas — passo a passo
📟 1. Display LCD 16×2 I²C
O módulo I²C tem quatro pinos, normalmente identificados como GND, VCC, SDA e SCL.
LCD I²C	Arduino UNO	Função
GND	GND	Terra
VCC	5V	Alimentação
SDA	A4	Dados I²C
SCL	A5	Clock I²C


       LCD 16x2 + I2C                ARDUINO UNO
      ┌────────────────┐            ┌─────────────┐
      │ GND ───────────────────────── GND         │
      │ VCC ───────────────────────── 5V          │
      │ SDA ───────────────────────── A4 (SDA)    │
      │ SCL ───────────────────────── A5 (SCL)    │
      └────────────────┘            └─────────────┘
💡 O código usa o endereço I²C 0x27. Se o LCD acender mas não mostrar caracteres, ajuste o contraste no potenciômetro do módulo e verifique o endereço (alguns usam 0x3F).

🔘 2. Botão de transmissão Morse
Ligue um terminal do botão ao D2 e o outro ao GND.
Botão	Arduino UNO
Terminal 1	D2
Terminal 2	GND


 ARDUINO UNO
 ┌────────────┐
 │ D2 ───────────────┐
 │                  [ BOTÃO ]
 │ GND ──────────────┘
 └────────────┘
O programa usa pinMode(BOTAO, INPUT_PULLUP), portanto não é necessário resistor externo: solto = HIGH, pressionado = LOW.
⚠️ Botões táteis de 4 pernas costumam ter dois pares de terminais já interligados internamente. Confira a disposição com um multímetro ou a documentação do botão para não conectar D2 e GND ao mesmo par permanente.

🔊 3. Buzzer passivo
Buzzer	Arduino UNO
+ (positivo)	D8
− (negativo)	GND


 ARDUINO UNO
 ┌────────────┐
 │ D8 ───────────── (+) BUZZER
 │ GND ──────────── (−) BUZZER
 └────────────┘
O buzzer toca enquanto você mantém o botão pressionado e também emite sinais curtos de navegação, acerto e erro. Use um buzzer passivo de baixa corrente; módulos ou dispositivos que exigem corrente elevada precisam de circuito de acionamento adequado, como transistor.
🧩 Esquema geral
                        ARDUINO UNO
                   ┌──────────────────┐
 LCD I2C  GND ─────┤ GND              │
 LCD I2C  VCC ─────┤ 5V               │
 LCD I2C  SDA ─────┤ A4               │
 LCD I2C  SCL ─────┤ A5               │
                   │                  │
 BOTÃO ────────────┤ D2               │
   └── GND ────────┤ GND              │
                   │                  │
 BUZZER (+) ───────┤ D8               │
 BUZZER (−) ───────┤ GND              │
                   └──────────────────┘
Todos os GNDs devem compartilhar o terra do Arduino. Faça as ligações com a alimentação desligada e confira a polaridade antes de conectar o USB.
🎮 Os 4 modos
Modo	Nome	Como funciona
1	✍️ Digitação	Digite letras e números livremente em Morse. O LCD acumula o texto.
2	🧠 Treino	Uma palavra de 3 a 5 letras aparece. Se errar, o LCD mostra a letra correta e sua sequência Morse; tente novamente.
3	🏆 Desafio	Complete palavras curtas. Uma palavra certa soma 1 acerto; errar uma letra soma 1 erro e passa à próxima palavra.
4	🔢 Numbers	Um número de 0 a 9 aparece. Transmita seu Morse; se errar, veja a dica e tente novamente.


🕹️ Como selecionar um modo
1. Ligue o Arduino e espere aparecer o menu.
2. Cada toque curto muda para o próximo modo, acompanhado de um beep.
3. Pare de tocar por 3 segundos: o modo exibido inicia automaticamente.
4. O botão passa a ser usado somente para transmitir Morse.
🚪 Como sair
- Modos 1, 2 e 4: dê 5 toques muito rápidos para voltar ao menu.
- Modo 3: dê 5 toques muito rápidos para encerrar e mostrar o placar A:... E:.... Um toque na tela do placar retorna ao menu.
⚠️ Limitação do botão único: o número 5 em Morse é ....., portanto cinco pontos rápidos podem ser confundidos com o comando de saída. Para transmitir 5, use intervalos um pouco maiores entre os pontos. Uma solução definitiva seria usar um segundo botão para o menu.

📟 Exemplos de telas
Menu:
┌────────────────┐
│MODO 4 NUMBERS  │
│AUTO EM 3 SEG   │
└────────────────┘
Dica no treino:
┌────────────────┐
│ERRO! LETRA: S  │
│DICA: ...       │
└────────────────┘
Treino de números:
┌────────────────┐
│NUMERO: 7       │
│Digite Morse    │
└────────────────┘
Placar do desafio:
┌────────────────┐
│DESAFIO ENCERR. │
│A:7 E:3         │
└────────────────┘
⏱️ Tempos e interpretação
Ação	Configuração padrão
Ponto (.)	Pressionar por menos de 400 ms
Traço (-)	Pressionar por 400 ms ou mais
Final de letra	Pausa de 850 ms
Espaço entre palavras (modo 1)	Pausa de 1800 ms
Seleção automática no menu	3000 ms sem toques
Limpar digitação livre	60 segundos sem nova letra


Esses tempos são ajustáveis nas constantes do início do sketch.
🔤 Tabela rápida de código Morse
Caractere	Morse	Caractere	Morse
A	.-	N	-.
B	-...	O	---
C	-.-.	P	.--.
D	-..	Q	--.-
E	.	R	.-.
F	..-.	S	...
G	--.	T	-
H	....	U	..-
I	..	V	...-
J	.---	W	.--
K	-.-	X	-..-
L	.-..	Y	-.--
M	--	Z	--..


🔢 Números
Número	Morse
0	-----
1	.----
2	..---
3	...--
4	....-
5	.....
6	-....
7	--...
8	---..
9	----.


💻 Instalação na Arduino IDE
1. Instale a Arduino IDE.
2. Abra o sketch do projeto (.ino).
3. Instale a biblioteca LiquidCrystal I2C compatível com LiquidCrystal_I2C lcd(0x27, 16, 2).
4. Em Ferramentas → Placa, selecione Arduino UNO.
5. Selecione a porta USB correta e clique em Carregar.
6. Abra o Monitor Serial em 9600 baud para acompanhar as letras reconhecidas.
🛠️ Solução de problemas
- LCD acende, mas não mostra texto: ajuste o contraste e confira SDA=A4, SCL=A5 e o endereço I²C.
- Botão não responde: confira D2, GND e os pares internos do botão de quatro pernas.
- Buzzer não toca: confira a polaridade, se é passivo e a ligação ao D8.
- Morse decodifica antes da hora: aumente PAUSA_LETRA para 1000 ou 1200 ms.
- Modo inicia sozinho: esse é o comportamento previsto; após 3 segundos sem tocar no menu, a seleção é automática.
- O número 5 encerra a sessão: diminua a velocidade dos pontos ou use um botão separado para sair.
Feito para experimentar, praticar e aprender comunicação por código Morse. 📻✨
