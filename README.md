Aqui está um exemplo de como você pode criar um jogo da cobrinha em Java usando a biblioteca Swing para criar uma interface gráfica. Este código é um jogo simples que você pode adaptar e usar em um repositório do GitHub. Lembre-se de que, para rodar o jogo em dispositivos móveis, você precisaria de uma abordagem diferente, como desenvolver um aplicativo Android. No entanto, este exemplo é para um aplicativo desktop.

### Código do Jogo da Cobrinha em Java

```java
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.KeyAdapter;
import java.awt.event.KeyEvent;
import java.util.LinkedList;
import java.util.Random;

public class SnakeGame extends JPanel implements ActionListener {
    private final int WIDTH = 600;
    private final int HEIGHT = 400;
    private final int DOT_SIZE = 10;
    private final int ALL_DOTS = (WIDTH * HEIGHT) / (DOT_SIZE * DOT_SIZE);
    private final LinkedList<Point> snake = new LinkedList<>();
    private Point food;
    private char direction = 'R';
    private boolean running = false;

    public SnakeGame() {
        setBackground(Color.BLACK);
        setFocusable(true);
        setPreferredSize(new Dimension(WIDTH, HEIGHT));
        addKeyListener(new KeyAdapter() {
            @Override
            public void keyPressed(KeyEvent e) {
                switch (e.getKeyCode()) {
                    case KeyEvent.VK_UP:
                        if (direction != 'D') direction = 'U';
                        break;
                    case KeyEvent.VK_DOWN:
                        if (direction != 'U') direction = 'D';
                        break;
                    case KeyEvent.VK_LEFT:
                        if (direction != 'R') direction = 'L';
                        break;
                    case KeyEvent.VK_RIGHT:
                        if (direction != 'L') direction = 'R';
                        break;
                }
            }
        });
        initGame();
    }

    private void initGame() {
        running = true;
        snake.clear();
        snake.add(new Point(5, 5));
        spawnFood();
        Timer timer = new Timer(100, this);
        timer.start();
    }

    private void spawnFood() {
        Random rand = new Random();
        food = new Point(rand.nextInt(WIDTH / DOT_SIZE), rand.nextInt(HEIGHT / DOT_SIZE));
        while (snake.contains(food)) {
            food = new Point(rand.nextInt(WIDTH / DOT_SIZE), rand.nextInt(HEIGHT / DOT_SIZE));
        }
    }

    @Override
    protected void paintComponent(Graphics g) {
        super.paintComponent(g);
        if (running) {
            g.setColor(Color.RED);
            g.fillRect(food.x * DOT_SIZE, food.y * DOT_SIZE, DOT_SIZE, DOT_SIZE);
            g.setColor(Color.GREEN);
            for (Point p : snake) {
                g.fillRect(p.x * DOT_SIZE, p.y * DOT_SIZE, DOT_SIZE, DOT_SIZE);
            }
        } else {
            showGameOver(g);
        }
        Toolkit.getDefaultToolkit().sync();
    }

    private void showGameOver(Graphics g) {
        g.setColor(Color.WHITE);
        g.setFont(new Font("Arial", Font.BOLD, 20));
        g.drawString("Game Over!", WIDTH / 2 - 50, HEIGHT / 2);
    }

    private void move() {
        Point head = snake.getFirst();
        Point newHead = new Point(head);

        switch (direction) {
            case 'U': newHead.y--; break;
            case 'D': newHead.y++; break;
            case 'L': newHead.x--; break;
            case 'R': newHead.x++; break;
        }

        if (newHead.equals(food)) {
            snake.addFirst(newHead);
            spawnFood();
        } else {
            snake.addFirst(newHead);
            snake.removeLast();
        }

        // Check for collisions
        if (newHead.x < 0 || newHead.x >= WIDTH / DOT_SIZE || newHead.y < 0 || newHead.y >= HEIGHT / DOT_SIZE || snake.subList(1, snake.size()).contains(newHead)) {
            running = false;
        }
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        if (running) {
            move();
        }
        repaint();
    }

    public static void main(String[] args) {
        JFrame frame = new JFrame("Snake Game");
        SnakeGame game = new SnakeGame();
        frame.add(game);
        frame.pack();
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setLocationRelativeTo(null);
        frame.setVisible(true);
    }
}
```

### Instruções para Configurar no GitHub

1. **Crie um novo repositório** no GitHub.
2. **Adicione os arquivos**: Crie um arquivo chamado `SnakeGame.java` e cole o código acima.
3. **Adicione um README**: Inclua um arquivo `README.md` com instruções sobre como compilar e executar o jogo. Você pode usar o seguinte texto:

   ```markdown
   # Jogo da Cobrinha em Java

   ## Como Executar

   1. Certifique-se de ter o Java Development Kit (JDK) instalado.
   2. Baixe o repositório.
   3. Abra o terminal e navegue até a pasta do projeto.
   4. Compile o jogo:
      ```
      javac SnakeGame.java
      ```
   5. Execute o jogo:
      ```
      java SnakeGame
      ```

   Use as teclas de seta para mover a cobra!
   ```

4. **Publicação**: Você pode usar GitHub Pages para adicionar uma página de apresentação, mas como este é um aplicativo Java, os usuários devem baixar e executar localmente.

### Observação
Este jogo é feito para desktop usando Java Swing e, para ser jogado em dispositivos móveis, seria necessário refatorar o código para um ambiente Android ou um aplicativo que suporte JavaScript. Se você precisar de ajuda com isso ou com algo mais específico, sinta-se à vontade para perguntar!
