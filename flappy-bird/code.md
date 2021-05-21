# this is the code for the flappy bird game
## it only showes the code for the main page, the whole code includes a bird and barrier classes 

``` flutter
import 'package:flappy/barriers.dart';
import 'package:flappy/bird.dart';
import 'package:flutter/material.dart';
import 'dart:async';
class HomePage extends StatefulWidget {
  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  static double yAxis = 0;
  double time = 0;
  double height = 0;
  double initialHeight = yAxis;
  bool gameStarted = false;
  double barrXFir = 1;
  double barrXSec = 2.5;
  int score = 0;
  int bestScore = 0;
  double cent = 0.26;
  bool isCollision(){
    if( !(yAxis >= -0.4 && yAxis <=0.4) && barrXFir > -cent && barrXFir < cent){
      return true;
    }
    else if( !(yAxis >= -0.5 && yAxis <=0.39) && barrXSec > -cent && barrXSec < cent)
      return true;
    return false;
  }

  void jump(){
    setState(() {
      initialHeight = yAxis;
      time = 0;
    });

  }
  void startGame(){
    score = 0;
    gameStarted = true;
    barrXFir = 1;
    barrXSec = 2.5;
    time = 0;
    height = 0;
    yAxis = 0;
    initialHeight = 0;
    Timer.periodic(Duration(milliseconds: 50), (timer) {
      time += 0.04;
      height = -4.9 * time * time + 2.4 * time;
      setState(() {
        yAxis = initialHeight - height;

      });
      if(barrXFir < -2.0){
        barrXFir += 3.5;
        score++;
      } else{
        barrXFir -= 0.05;
      }

      if(barrXSec < -2.0){
        barrXSec += 3.5;
        score++;
      } else{
        barrXSec -= 0.05;
      }

      if(yAxis > 1 ||isCollision()) {
        timer.cancel();
        gameStarted = false;
        showGameOver();
        setState(() {
          if(score>bestScore)
            bestScore = score;
        });

      }
    });
  }
  void showGameOver(){
    showDialog(context: context,
        builder: (BuildContext context){
          return AlertDialog(
            backgroundColor: Colors.green[600],
            title: Text('Game Over!', style: TextStyle(color: Colors.white60, fontSize: 35)),
            content: Text('Your Score:   ' + score.toString(), style: TextStyle(color: Colors.white60, fontSize: 25)),
            actions: <Widget>[
              TextButton(onPressed: (){
                startGame();
                Navigator.of(context).pop();
              }, child: Text('Play Again',style: TextStyle(color: Colors.white60, fontSize: 18)))
            ],
          );
        }
    );
  }
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Column(
        children: [
          Expanded(
            flex: 2,
            child: Stack(
                children: [
                  GestureDetector(
                    onTap: (){
                      if(gameStarted){
                        jump();
                      }
                      else{
                        startGame();
                      }
                    },
                    child: AnimatedContainer(
                      alignment: Alignment(0,yAxis),
                      duration: Duration(milliseconds: 0),
                      color: Colors.lightBlue,
                      child: Bird(),
                    ),
                  ),
                  Container(
                    alignment: Alignment(0,0.4),
                    child: gameStarted ? Text("") :
                    Text("S T A R T  G A M E", style: TextStyle(color: Colors.white60, fontSize: 20),),
                  ),
                  AnimatedContainer(
                    alignment: Alignment(barrXFir,1.5),
                      duration: Duration(milliseconds: 0),
                      child:  Barrier(barrierSize: 200.0),
                  ),
                  AnimatedContainer(
                    alignment: Alignment(barrXFir,-1.5),
                    duration: Duration(milliseconds: 0),
                    child:  Barrier(barrierSize: 200.0),
                  ),
                  AnimatedContainer(
                    alignment: Alignment(barrXSec,1.4),
                    duration: Duration(milliseconds: 0),
                    child:  Barrier(barrierSize: 200.0),
                  ),
                  AnimatedContainer(
                    alignment: Alignment(barrXSec,-1.8),
                    duration: Duration(milliseconds: 0),
                    child:  Barrier(barrierSize: 200.0),
                  )
                ],
              )
            ),
          Expanded(
            child: Container(
              color: Colors.green,
              child: Row(
                mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                children: [
                  Column(
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: [
                      Text('SCORE: ', style: TextStyle(color: Colors.brown, fontSize: 30),),
                      Text(score.toString(), style: TextStyle(color: Colors.brown, fontSize: 30),)
                    ],
                  ),
                  Column(
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: [
                      Text('TOP SCORE: ', style: TextStyle(color: Colors.brown, fontSize: 30),),
                      Text(bestScore.toString(), style: TextStyle(color: Colors.brown, fontSize: 30),)
                    ],
                  )
                ],
              ),
            ),
          ),

        ],  
     ),
    );
  }
}


