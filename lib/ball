import 'package:flutter/material.dart';
import 'dart:async';
import 'dart:math';

void main() {
  runApp(const BouncingBallGame());
}

class BouncingBallGame extends StatelessWidget {
  const BouncingBallGame({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      debugShowCheckedModeBanner: false,
      home: GameScreen(),
    );
  }
}

class GameScreen extends StatefulWidget {
  const GameScreen({super.key});

  @override
  State<GameScreen> createState() => _GameScreenState();
}

class _GameScreenState extends State<GameScreen> {
  double ballX = 0;
  double ballY = 0;
  double ballXSpeed = 0.02;
  double ballYSpeed = 0.02;

  double paddleX = 0;
  double paddleWidth = 0.3;
  int score = 0;

  late Timer gameTimer;

  @override
  void initState() {
    super.initState();
    startGame();
  }

  void startGame() {
    gameTimer = Timer.periodic(const Duration(milliseconds: 16), (timer) {
      setState(() {
        updateBallPosition();
        checkCollision();
      });
    });
  }

  void updateBallPosition() {
    ballX += ballXSpeed;
    ballY += ballYSpeed;

    // Ball collision with left and right walls
    if (ballX >= 1 || ballX <= -1) {
      ballXSpeed = -ballXSpeed;
    }

    // Ball collision with top wall
    if (ballY <= -1) {
      ballYSpeed = -ballYSpeed;
    }

    // Ball falls below paddle (Game Over)
    if (ballY >= 1) {
      gameTimer.cancel();
      showGameOverDialog();
    }
  }

  void checkCollision() {
    // Ball collision with paddle
    if (ballY >= 0.9 &&
        ballX >= paddleX - paddleWidth / 2 &&
        ballX <= paddleX + paddleWidth / 2) {
      ballYSpeed = -ballYSpeed;
      score++;
    }
  }

  void movePaddle(DragUpdateDetails details) {
    setState(() {
      paddleX += details.delta.dx / MediaQuery.of(context).size.width * 2;
      if (paddleX - paddleWidth / 2 < -1) {
        paddleX = -1 + paddleWidth / 2;
      } else if (paddleX + paddleWidth / 2 > 1) {
        paddleX = 1 - paddleWidth / 2;
      }
    });
  }

  void resetGame() {
    setState(() {
      ballX = 0;
      ballY = 0;
      ballXSpeed = 0.02;
      ballYSpeed = 0.02;
      paddleX = 0;
      score = 0;
    });
    startGame();
  }

  void showGameOverDialog() {
    showDialog(
      context: context,
      barrierDismissible: false,
      builder: (context) => AlertDialog(
        title: const Text("Game Over"),
        content: Text("Score: $score"),
        actions: [
          TextButton(
            onPressed: () {
              Navigator.of(context).pop();
              resetGame();
            },
            child: const Text("Play Again"),
          ),
        ],
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onHorizontalDragUpdate: movePaddle,
      child: Scaffold(
        backgroundColor: Colors.black,
        body: Center(
          child: Stack(
            children: [
              // Ball
              Positioned(
                top: (MediaQuery.of(context).size.height / 2) * (ballY + 1),
                left: (MediaQuery.of(context).size.width / 2) * (ballX + 1),
                child: Container(
                  width: 20,
                  height: 20,
                  decoration: const BoxDecoration(
                    color: Colors.red,
                    shape: BoxShape.circle,
                  ),
                ),
              ),
              // Paddle
              Positioned(
                bottom: 50,
                left: (MediaQuery.of(context).size.width / 2) *
                    (paddleX + 1 - paddleWidth / 2),
                child: Container(
                  width: MediaQuery.of(context).size.width * paddleWidth,
                  height: 20,
                  decoration: BoxDecoration(
                    color: Colors.blue,
                    borderRadius: BorderRadius.circular(10),
                  ),
                ),
              ),
              // Score Display
              Positioned(
                top: 40,
                left: 20,
                child: Text(
                  "Score: $score",
                  style: const TextStyle(
                    color: Colors.white,
                    fontSize: 24,
                    fontWeight: FontWeight.bold,
                  ),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
