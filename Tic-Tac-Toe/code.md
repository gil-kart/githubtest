
```fluter
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget{
  @override
  Widget build(BuildContext context) {

    return MaterialApp(
      home: HomePage(),
    );
  }
}

class HomePage extends StatefulWidget{
  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  int numberOfSquares = 9;
  final List <String> board = ['','','','','','','','',''];
  var turn = 0;
  void startGame(){
    int i;
    setState(() {
      for(i=0; i<9 ; i++)
        board[i]= '';
    });

  }

  void _showGameOver(String winner){
    showDialog(context: context,
        builder: (BuildContext context){
          return AlertDialog(
            backgroundColor: Colors.cyan,
            title: Text('Game Over!',  style: TextStyle(color: Colors.white70, fontSize: 40)),
            content: Text('The Winner is:   ' + winner,style: TextStyle(color: Colors.white70, fontSize: 30)),

            actions: <Widget>[
              TextButton(onPressed: (){
                startGame();
                Navigator.of(context).pop();
              }, child: Text('Play Again',style: TextStyle(color: Colors.orangeAccent, fontSize: 26)))
            ],
          );
        }
    );
  }


  void putShape(int ind){
      setState(() {
        if(turn == 0) {
          board[ind] = 'x';
          checkGameOver();
          turn = 1;
        }
        else{
          board[ind] = 'o';
          checkGameOver();
          turn = 0;
        }
      });
  }
  void checkGameOver(){
    int i;
    for(i=0; i<=6; i+=3){ // if line
      if(board[i] == board[i+1] && board[i+1] == board[i+2] && (board[i] == 'o'||board[i] == 'x'))
        _showGameOver(board[i]);
    }
    for(i=0;i<3;i++){
      if(board[i] == board[i+3] && board[i+3] == board[i+6] && (board[i] == 'o'||board[i] == 'x'))
        _showGameOver(board[i]);
    }
    i=0;
    if(board[i] == board[i+4] && board[i+4] == board[i+8] && (board[i] == 'o'||board[i] == 'x'))
      _showGameOver(board[i]);
    else if(board[2] == board[4] && board[4] == board[6] && (board[2] == 'o'||board[2] == 'x'))
      _showGameOver(board[2]);
  }
  @override
  Widget build(BuildContext context) {
      return Scaffold(
        backgroundColor: Colors.cyan,
        body: Column(
          children: <Widget>[
        Expanded(child: GestureDetector(
               child: Container(
               child: GridView.builder(
                 physics: NeverScrollableScrollPhysics(),
                 itemCount: numberOfSquares,
                 gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
                     crossAxisCount: 3),
                   itemBuilder: (BuildContext context, int index){
                   //----------------------
                     int i=0;
                     for(i=0;i<9;i++){
                       if(index == i){
                         return Center(
                           child: Container(
                             padding: EdgeInsets.all(0),
                             child: ClipRRect(
                               borderRadius: BorderRadius.circular(3),
                               child: Container(
                                 margin: const EdgeInsets.all(10.0),
                                 //color: Colors.amber[600],
                                 width: 98.0,
                                 height: 98.0,
                                 child: ElevatedButton(onPressed: ()=>putShape(i),child: Text(board[i], style: TextStyle(color: Colors.orange, fontSize: 80, fontFamily: 'Optima')),

                                 ),
                                 color: Colors.greenAccent,
                               ),
                             ),
                           ),
                         );
                       }
                     }

                       return Center(
                         child: Container(
                           padding: EdgeInsets.all(0),
                           child: ClipRRect(
                             borderRadius: BorderRadius.circular(3),
                             child: Container(
                               child: ElevatedButton(onPressed: ()=>putShape(9),child: Text(board[9], style: TextStyle(color: Colors.orange, fontSize: 40)),
                               ),
                               color: Colors.greenAccent,
                             ),
                           ),
                         ),
                       );

                   }
                )
               )
             )
            )
          ],
        ),
      );
  }
}
