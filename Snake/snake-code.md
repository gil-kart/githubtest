# this is the code for my snake game:

```fluter
import 'package:flutter/material.dart';
import 'dart:async';
import 'dart:math';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget{
  @override
  Widget build(BuildContext context) {

    return MaterialApp(
      home: HomePage(),
    );
    throw UnimplementedError();
  }
}

class HomePage extends StatefulWidget{
  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  var numberOfSquares = 760;
  static List<int> snakePosition = [45, 65, 85, 105, 125];
  static var randomNumber = Random();
  int food = randomNumber.nextInt(400);
  void generateFood(){
    food = randomNumber.nextInt(500);
  }
  void startGame(){
    snakePosition = [45, 65, 85, 105, 125];
    const duration = const Duration(milliseconds: 400);
    Timer.periodic(duration, (Timer timer) {
        updateSnake();
        if(gameOver()){
          timer.cancel();
          _showGameOver();
        }
    });
  }
  void _showGameOver(){
    showDialog(context: context,
    builder: (BuildContext context){
          return AlertDialog(
            title: Text('Game Over!'),
            content: Text('Your Score: ' + snakePosition.length.toString()),
            actions: <Widget>[
              TextButton(onPressed: (){
                startGame();
                Navigator.of(context).pop();
              }, child: Text('Play Again'))
            ],
          );
        }
    );
  }
  bool gameOver(){
    int count = 0;
    for(int i=0; i<snakePosition.length;i++){
      count=0;
      for(int j = 0; j < snakePosition.length; j++){
        if(snakePosition[i] == snakePosition[j])
          count += 1;
      }
      if(count == 2)
        return true;
    }
    return false;
  }
  var direction = 'down';
  void updateSnake(){
    setState(() {
      switch(direction){
        case 'down':
          if(snakePosition.last >740){
            snakePosition.add(snakePosition.last + 20 - 760);
          }
          else{
            snakePosition.add(snakePosition.last + 20);
          }
          break;

        case 'up':
          if(snakePosition.last < 20){
            snakePosition.add(snakePosition.last - 20 + 760);
          }
          else{
            snakePosition.add(snakePosition.last - 20);
          }
          break;

        case 'left':
          if(snakePosition.last % 20 == 0){
            snakePosition.add(snakePosition.last -1 + 20);
          }
          else{
            snakePosition.add(snakePosition.last -1);
          }
          break;
        case 'right':
          if((snakePosition.last + 1) % 20 == 0){
            snakePosition.add(snakePosition.last + 1 - 20);
          }
          else{
            snakePosition.add(snakePosition.last + 1);
          }
          break;
        default:
      }
      if(snakePosition.last == food){
        generateFood();
      } else{
        snakePosition.removeAt(0);
      }
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.blueGrey,
      body: Column(
        children: <Widget>[
          Expanded(child: GestureDetector(
              onVerticalDragUpdate: (details){
                if(direction != 'up' && details.delta.dy > 0){
                  direction = 'down';
                 } else if(direction != 'down' &&  details.delta.dy < 0){
                  direction = 'up';
                }
              },
              onHorizontalDragUpdate: (details){
                if(direction != 'left' && details.delta.dx > 0){
                  direction = 'right';
                } else if(direction != 'right' &&  details.delta.dx < 0){
                  direction = 'left';
                }
              } ,


            child: Container(
              child: GridView.builder(
                  physics: NeverScrollableScrollPhysics(),
                  itemCount: numberOfSquares,
                  gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
                    crossAxisCount: 20),
                  itemBuilder: (BuildContext context, int index){
                      if(snakePosition.contains(index)){
                        return Center(
                          child: Container(
                            padding: EdgeInsets.all(0),
                            child: ClipRRect(
                              borderRadius: BorderRadius.circular(3),
                              child: Container(
                                color: Colors.greenAccent,
                              ),
                            ),
                          ),
                        );
                      }
                      if(index == food){
                        return Container(
                          padding: EdgeInsets.all(0),
                          child: ClipRRect(
                          borderRadius: BorderRadius.circular(10),

                            child: Container(
                              color: Colors.pink,
                            ),
                          )
                        );
                      }
                      else{
                        return Container(
                            padding: EdgeInsets.all(0),
                            child: ClipRRect(
                              borderRadius: BorderRadius.circular(1),
                              child: Container(
                                color: Colors.blueGrey,
                              ),
                            )
                        );
                      }
                  },

               ),
              )
            )
          ),
          Padding(
            padding: const EdgeInsets.only(bottom: 20.0, left: 20.0, right: 20.0),
            child: Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: <Widget>[
                GestureDetector(
                  onTap: startGame,
                  child: Text('S T A R T',
                    style: TextStyle(color: Colors.white, fontSize: 20),
                  ),
                )

              ],
            ),
          )
        ],

      ),
    );

  }
}

