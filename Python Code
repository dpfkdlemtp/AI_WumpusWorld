Wumpus World Team 11

python code

```python
# -*- coding: utf-8 -*-
# '' Unknown B Wall S Safe G Gold D DeadWumpus W Wumpus P Pit S StartPoint 0 Blank V Visit
# Glitter 1  Stench 2 Breeze 4 Bump 8 Scream 16
import random
import sys
from PyQt5.QtWidgets import *
from PyQt5.QtGui import *

Goal = 0
Try = 1
cur_x = 1
cur_y = 1
cur_d = 'E'
Gold = 0
Win = 0
Dead = 0
arrow = 3
isolate = 0
Start = 0
Wumpus = 0

array1 = [[0] * 7 for i in range(7)]  # 지형지물 설정
array2 = [[0] * 7 for i in range(7)]  # 센서 설정
array3 = [['U'] * 7 for i in range(7)]  # 에이전트 상태
wumarr = [[0] * 5 for i in range(5)]  # Dead Wumpus 위치

#GUI 화면 구성
class MyWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        global Gold, Try, cur_x, cur_y, cur_d, Goal, Win, arrow, isolate, Dead, Wumpus
        self.setGeometry(400, 220, 900, 640)  # 실행 시 위치, 크기
        self.setWindowTitle("Wumpus World")  # 프로그램 타이틀
        self.setWindowIcon(QIcon("wumpus.png"))  # 프로그램 아이콘

        self.select = QPushButton("Agent 행동", self)
        self.select.move(340, 20)
        self.select.clicked.connect(self.act_clicked)
        self.select.resize(100, 40)

        self.select = QPushButton("새로 시작", self)
        self.select.move(480, 20)
        self.select.clicked.connect(self.new_clicked)
        self.select.resize(100, 40)

        self.label1 = QLabel("Agent State", self)
        self.label1.move(200, 60)
        self.label1.resize(100, 40)

        self.label2 = QLabel("Wumpus World:", self)
        self.label2.move(620, 60)
        self.label2.resize(100, 40)

        self.label3 = QLabel("현재 시도:", self)
        self.label3.move(50, 500)
        self.label3.resize(100, 40)

        self.label9 = QLabel("1", self)
        self.label9.move(120, 500)
        self.label9.resize(100, 40)

        self.label3 = QLabel("현재 위치:", self)
        self.label3.move(50, 520)
        self.label3.resize(100, 40)

        self.label4 = QLabel(str(cur_y) + "," + str(cur_x), self)
        self.label4.move(120, 520)
        self.label4.resize(100, 40)

        self.label5 = QLabel("현재 방향:", self)
        self.label5.move(50, 540)
        self.label5.resize(100, 40)

        self.label6 = QLabel(str(cur_d), self)
        self.label6.move(120, 540)
        self.label6.resize(100, 40)

        self.label7 = QLabel("Action  :", self)
        self.label7.move(50, 560)
        self.label7.resize(100, 40)

        self.label10 = QLabel("", self)
        self.label10.move(110, 560)
        self.label10.resize(200, 40)

        self.label8 = QLabel("Arrow   :", self)
        self.label8.move(50, 580)
        self.label8.resize(100, 40)

        self.label11 = QLabel("", self)
        self.label11.move(110, 580)
        self.label11.resize(100, 40)

        self.label12 = QLabel("Sensor :", self)
        self.label12.move(50, 600)
        self.label12.resize(100, 40)

        self.label13 = QLabel("", self)
        self.label13.move(110, 600)
        self.label13.resize(150, 40)

        self.agentstate = QTableWidget(self)
        self.agentstate.move(40, 100)
        self.agentstate.resize(400, 400)
        self.agentstate.setRowCount(6)
        self.agentstate.setColumnCount(6)
        self.HHeaderLabels = [' ', '1', '2', '3', '4', ' ']
        self.VHeaderLabels = [' ', '4', '3', '2', '1', ' ']
        self.agentstate.setHorizontalHeaderLabels(self.HHeaderLabels)
        self.agentstate.setVerticalHeaderLabels(self.VHeaderLabels)
        self.agentstate.setColumnWidth(0, self.width() * 1 // 16)
        self.agentstate.setColumnWidth(1, self.width() * 1 // 16)
        self.agentstate.setColumnWidth(2, self.width() * 1 // 16)
        self.agentstate.setColumnWidth(3, self.width() * 1 // 16)
        self.agentstate.setColumnWidth(4, self.width() * 1 // 16)
        self.agentstate.setColumnWidth(5, self.width() * 1 // 16)
        self.agentstate.setRowHeight(0, self.height() * 1 // 11)
        self.agentstate.setRowHeight(1, self.height() * 1 // 11)
        self.agentstate.setRowHeight(2, self.height() * 1 // 11)
        self.agentstate.setRowHeight(3, self.height() * 1 // 11)
        self.agentstate.setRowHeight(4, self.height() * 1 // 11)
        self.agentstate.setRowHeight(5, self.height() * 1 // 11)

        self.wumpusworld = QTableWidget(self)
        self.wumpusworld.move(480, 100)
        self.wumpusworld.resize(400, 400)
        self.wumpusworld.setRowCount(6)
        self.wumpusworld.setColumnCount(6)
        self.HHeaderLabels = [' ', '1', '2', '3', '4', ' ']
        self.VHeaderLabels = [' ', '4', '3', '2', '1', ' ']
        self.wumpusworld.setHorizontalHeaderLabels(self.HHeaderLabels)
        self.wumpusworld.setVerticalHeaderLabels(self.VHeaderLabels)
        self.wumpusworld.setColumnWidth(0, self.width() * 1 // 16)
        self.wumpusworld.setColumnWidth(1, self.width() * 1 // 16)
        self.wumpusworld.setColumnWidth(2, self.width() * 1 // 16)
        self.wumpusworld.setColumnWidth(3, self.width() * 1 // 16)
        self.wumpusworld.setColumnWidth(4, self.width() * 1 // 16)
        self.wumpusworld.setColumnWidth(5, self.width() * 1 // 16)
        self.wumpusworld.setRowHeight(0, self.height() * 1 // 11)
        self.wumpusworld.setRowHeight(1, self.height() * 1 // 11)
        self.wumpusworld.setRowHeight(2, self.height() * 1 // 11)
        self.wumpusworld.setRowHeight(3, self.height() * 1 // 11)
        self.wumpusworld.setRowHeight(4, self.height() * 1 // 11)
        self.wumpusworld.setRowHeight(5, self.height() * 1 // 11)

    # 새로 시작 버튼 구현
    def new_clicked(self):
        global Gold, Try, cur_x, cur_y, cur_d, Goal, Win, arrow, isolate, Dead, Start, Wumpus
        Goal = 0
        Try = 1
        cur_x = 1
        cur_y = 1
        cur_d = 'E'
        Gold = 0
        Win = 0
        Dead = 0
        arrow = 3
        isolate = 0
        Start = 0

        # make World
        Set_Wumpus_World(array1, array2, array3)
        # Sensor Setting
        Set_State(array1, array2)

        for i in range(6):
            for j in range(6):
                self.wumpusworld.setItem(i, j, QTableWidgetItem(array1[5 - i][j]))

        for i in range(6):
            for j in range(6):
                self.agentstate.setItem(i, j, QTableWidgetItem(array3[5 - i][j]))

        QMessageBox.about(self, "알림", "Wumpus World가 구성되었습니다.")

    # Agent 행동 버튼 구현
    def act_clicked(self):
        global Gold, Try, cur_x, cur_y, cur_d, Goal, Win, arrow, isolate, Dead, Start, Wumpus
        self.label9.setText(str(Try))
        self.label11.setText(str(arrow))
        self.label6.setText(str(cur_d))
        # Gold 획득 후 시작점으로 돌아온 경우
        if Win == 1:
            self.label13.setText(str("Safe"))
            self.label10.setText(str("Climb"))
            QMessageBox.about(self, "알림", "축하드립니다!")
        # 죽은 경우 상태 유지
        elif Dead == 1:
            for i in range(0, 5):
                for j in range(0, 5):
                    if array3[i][j] == 'V':
                        array3[i][j] = 'S'
            while Wumpus > 0:
                array1[wumarr[Wumpus - 1][0]][wumarr[Wumpus - 1][1]] = 'W'
                array3[wumarr[Wumpus - 1][0]][wumarr[Wumpus - 1][1]] = 'W'
                Wumpus -= 1
            self.label10.setText(str("Dead"))
            Dead = 0
            Try += 1
            QMessageBox.about(self, "알림", "Agent가 죽었습니다.")
            cur_x = 1
            cur_y = 1
            cur_d = 'E'
            Gold = 0
            arrow = 3
            isolate = 0
            Start = 0
            self.label4.setText(str(cur_x) + "," + str(cur_y))
            self.label6.setText(str(cur_d))
            return
        # 처음 시작
        elif Start == 0:
            self.label13.setText(str("Safe"))
            self.label10.setText(str("Go Forward"))
            QMessageBox.about(self, "알림", "Agent가 (1,1) 에서 E방향으로 출발합니다.")
            array3[cur_y][cur_x] = 'A'
            Start = 1
            Wumpus = 0
            for i in range(6):
                for j in range(6):
                    self.agentstate.setItem(i, j, QTableWidgetItem(array3[5 - i][j]))
                    self.wumpusworld.setItem(i, j, QTableWidgetItem(array1[5 - i][j]))

        # 금을 획득 하지 못한 경우 탐색
        elif Gold == 0:
            array3[cur_y][cur_x] = 'V'
            self.label10.setText(str("Go Forward"))
            cur_y, cur_x, cur_d = Move(cur_y, cur_x, cur_d)  # move

            # 센서 설정
            if(array1[cur_y+1][cur_x] == 'W' or array1[cur_y][cur_x-1] == 'W' or array1[cur_y][cur_x+1] == 'W' or
                    array1[cur_y-1][cur_x] == 'W'):
                self.label13.setText(str("Stench"))
                if (array1[cur_y + 1][cur_x] == 'P' or array1[cur_y][cur_x - 1] == 'P' or array1[cur_y][
                    cur_x + 1] == 'P' or array1[cur_y - 1][cur_x] == 'P'):
                    self.label13.setText(str("Stench and Breeze"))
            elif (array1[cur_y + 1][cur_x] == 'P' or array1[cur_y][cur_x - 1] == 'P' or array1[cur_y][
                cur_x + 1] == 'P' or array1[cur_y - 1][cur_x] == 'P'):
                self.label13.setText(str("Breeze"))
            else:
                self.label13.setText(str("Safe"))

            # 위치 및 방향 설정
            self.label4.setText(str(cur_x) + "," + str(cur_y))
            self.label6.setText(str(cur_d))

            # 고립된 경우
            if isolate > 3:
                for i in range(isolate // 4):
                    self.label10.setText(str("Go Backward"))
                    cur_y, cur_x, cur_d = Back_Move(cur_y, cur_x, cur_d)
                cur_y, cur_x, cur_d = Back_Move(cur_y, cur_x, cur_d)
                self.label10.setText(str("Go Backward and Turn Left"))
                self.label4.setText(str(cur_x) + "," + str(cur_y))
                cur_d = Turn_Left(cur_d)
                self.label6.setText(str(cur_d))
                if array3[cur_y][cur_x] == 'V':
                    isolate = 0
                array3[cur_y][cur_x] = 'A'
                for i in range(6):
                    for j in range(6):
                        self.agentstate.setItem(i, j, QTableWidgetItem(array3[5 - i][j]))
                return

            # 재방문을 하는 경우
            elif array3[cur_y][cur_x] == 'V':
                isolate += 1
                cur_y, cur_x, cur_d = Back_Move(cur_y, cur_x, cur_d)
                cur_d = Turn_Left(cur_d)
                self.label4.setText(str(cur_x) + "," + str(cur_y))
                self.label6.setText(str(cur_d))
                for i in range(6):
                    for j in range(6):
                        self.agentstate.setItem(i, j, QTableWidgetItem(array3[5 - i][j]))
                return

            # Wumpus의 위치를 알고, 화살이 있는 경우
            elif array3[cur_y][cur_x] == 'W' and arrow > 0:
                self.label10.setText(str("Shoot"))
                self.label13.setText(str("Scream"))
                QMessageBox.about(self, "알림", "Shoot!")
                arrow -= 1
                array1[cur_y][cur_x] = 'D'
                array3[cur_y][cur_x] = ''
                wumarr[Wumpus][0] = cur_y
                wumarr[Wumpus][1] = cur_x
                Wumpus += 1
                cur_y, cur_x, cur_d = Back_Move(cur_y, cur_x, cur_d)
                array3[cur_y][cur_x] = 'A'
                for i in range(6):
                    for j in range(6):
                        self.agentstate.setItem(i, j, QTableWidgetItem(array3[5 - i][j]))
                        self.wumpusworld.setItem(i, j, QTableWidgetItem(array1[5 - i][j]))
                return
            # Wumpus 위치를 알고 화살이 없는 경우 또는 Pits의 위치를 알거나 벽의 위치를 아는 경우
            elif array3[cur_y][cur_x] == 'P' or array3[cur_y][cur_x] == 'W' or array3[cur_y][cur_x] == 'B':
                if array3[cur_y][cur_x] == 'P':
                    self.label13.setText(str("Breeze"))
                elif array3[cur_y][cur_x] == 'W':
                    self.label13.setText(str("Stench"))
                isolate += 1
                cur_y, cur_x, cur_d = Back_Move(cur_y, cur_x, cur_d)
                cur_d = Turn_Left(cur_d)
                array3[cur_y][cur_x] = 'A'
                self.label6.setText(str(cur_d))
                self.label4.setText(str(cur_x) + "," + str(cur_y))
                for i in range(6):
                    for j in range(6):
                        self.agentstate.setItem(i, j, QTableWidgetItem(array3[5 - i][j]))
                return
            else:
                # Gliiter 감지
                if array2[cur_y][cur_x] % 2 == 1:
                    if Gold == 0:
                        array3[cur_y][cur_x] = 'A'
                        self.label13.setText(str("Glitter"))
                        self.label10.setText(str("Grab"))
                        for i in range(6):
                            for j in range(6):
                                self.agentstate.setItem(i, j, QTableWidgetItem(array3[5 - i][j]))
                        QMessageBox.about(self, "알림", "Grab!")
                        Gold = 1
                        self.label10.setText(str("Turn 180'"))
                        cur_d = Turn_Left(cur_d)
                        cur_d = Turn_Left(cur_d)
                        self.label6.setText(str(cur_d))
                        return
                # Pits 밟고 죽음
                elif array1[cur_y][cur_x] == 'P':
                    self.label13.setText(str("Breeze"))
                    QMessageBox.about(self, "알림", "Pits!")
                    array3[cur_y][cur_x] = 'P'
                    Dead = 1
                    for i in range(6):
                        for j in range(6):
                            self.agentstate.setItem(i, j, QTableWidgetItem(array3[5 - i][j]))
                    return
                # Wumpus 밟고 죽음
                elif array1[cur_y][cur_x] == 'W' and array3[cur_y][cur_x] == 0:
                    self.label13.setText(str("Stench"))
                    QMessageBox.about(self, "알림", "Wumpus!")
                    array3[cur_y][cur_x] = 'W'
                    Dead = 1
                    for i in range(6):
                        for j in range(6):
                            self.agentstate.setItem(i, j, QTableWidgetItem(array3[5 - i][j]))
                    return
                # Bump 감지
                elif array2[cur_y][cur_x] > 7 and array1[cur_y][cur_x] == 'B':  # Bump
                    isolate += 1
                    self.label13.setText(str("Bump"))
                    QMessageBox.about(self, "알림", "Bump!")
                    array3[cur_y][cur_x] = 'B'
                    self.label10.setText(str("Go Backward and Turn Left"))
                    cur_y, cur_x, cur_d = Back_Move(cur_y, cur_x, cur_d)
                    cur_d = Turn_Left(cur_d)
                    self.label4.setText(str(cur_x) + "," + str(cur_y))
                    array3[cur_y][cur_x] = 'A'
                    self.label6.setText(str(cur_d))
                    for i in range(6):
                        for j in range(6):
                            self.agentstate.setItem(i, j, QTableWidgetItem(array3[5 - i][j]))
                    return
                else:
                    array3[cur_y][cur_x] = 'A'
                    isolate = 0
                    for i in range(6):
                        for j in range(6):
                            self.agentstate.setItem(i, j, QTableWidgetItem(array3[5 - i][j]))
                    return
            for i in range(6):
                for j in range(6):
                    self.agentstate.setItem(i, j, QTableWidgetItem(array3[5 - i][j]))
            return
        # 금을 획득한 경우
        else:
            # 출발점에 도착
            if array1[cur_y][cur_x] == 'S':
                array3[cur_y][cur_x] = 'R'
                Win = 1
                for i in range(6):
                    for j in range(6):
                        self.agentstate.setItem(i, j, QTableWidgetItem(array3[5 - i][j]))
                self.label10.setText(str("Climb"))
                QMessageBox.about(self, "알림", "Climb!")
                return
            # 출발점이 아닌 경우
            else:
                if array3[cur_y][cur_x] != 'R':
                    array3[cur_y][cur_x] = 'R'
                    for i in range(6):
                        for j in range(6):
                            self.agentstate.setItem(i, j, QTableWidgetItem(array3[5 - i][j]))
                    self.label13.setText(str("Safe"))
                    return
                self.label10.setText(str("Go Forward"))
                cur_y, cur_x, cur_d = Move(cur_y, cur_x, cur_d) # move
                # 센서 감지
                if (array1[cur_y + 1][cur_x] == 'W' or array1[cur_y][cur_x - 1] == 'W' or array1[cur_y][
                    cur_x + 1] == 'W' or
                        array1[cur_y - 1][cur_x] == 'W'):
                    self.label13.setText(str("Stench"))
                    if (array1[cur_y + 1][cur_x] == 'P' or array1[cur_y][cur_x - 1] == 'P' or array1[cur_y][
                        cur_x + 1] == 'P' or array1[cur_y - 1][cur_x] == 'P'):
                        self.label13.setText(str("Stench and Breeze"))
                if (array1[cur_y + 1][cur_x] == 'P' or array1[cur_y][cur_x - 1] == 'P' or array1[cur_y][
                    cur_x + 1] == 'P' or array1[cur_y - 1][cur_x] == 'P'):
                    self.label13.setText(str("Breeze"))

                self.label4.setText(str(cur_x) + "," + str(cur_y))
                if array3[cur_y][cur_x] != 'V':  # 오지 않았던 경로
                    cur_y, cur_x, cur_d = Back_Move(cur_y, cur_x, cur_d)
                    array3[cur_y][cur_x] = 'R'
                    self.label10.setText(str("Turn Right"))
                    cur_d = Turn_Right(cur_d)
                    self.label6.setText(str(cur_d))
                    self.label4.setText(str(cur_x) + "," + str(cur_y))
                else:
                    array3[cur_y][cur_x] = 'R'
                    self.label13.setText(str("Safe"))
                for i in range(6):
                    for j in range(6):
                        self.agentstate.setItem(i, j, QTableWidgetItem(array3[5 - i][j]))
                return


def Set_Wumpus_World(array1, array2, array3): # Wumpus World 구성
    Gold = 0
    for i in range(6):
        for j in range(6):
            array1[i][j] = ''
    for i in range(6):
        for j in range(6):
            array2[i][j] = 0
    for i in range(6):
        for j in range(6):
            array3[i][j] = 0

    # make wall
    for i in range(6):
        array1[i][0] = 'B'
        array1[i][5] = 'B'
        array1[5][i] = 'B'
        array1[0][i] = 'B'

    # Start Point
    array1[1][1] = 'S'

    # Set Gold, Wumpus, Pit
    while not Gold:
        for i in range(1, 5):
            for j in range(1, 5):
                if i + j > 3:
                    rand = random.randint(1, 100) # 15% 확률로 Wumpus 생성
                    if rand < 16:
                        array1[i][j] = 'W'
                    else:
                        rand = random.randint(1, 100) # 15% 확률로 Pits 생성
                        if rand < 16:
                            array1[i][j] = 'P'
                if Gold == 0:
                    if i + j > 3:
                        rand = random.randint(1, 100) # 15% 확률로 Gold 생성
                        if rand < 16:
                            array1[i][j] = 'G'
                            Gold = 1


def Move(y, x, d): # 바라보는 방향으로 이동
    if d == 'E':
        x = x + 1
    if d == 'W':
        x = x - 1
    if d == 'N':
        y = y + 1
    if d == 'S':
        y = y - 1
    return y, x, d


def Back_Move(y, x, d): # 바라보는 방향 반대로 이동
    if d == 'E':
        x -= 1
    elif d == 'N':
        y -= 1
    elif d == 'W':
        x += 1
    elif d == 'S':
        y += 1
    return y, x, d


def Turn_Left(d): # 왼쪽으로 회전
    print("Turn Left")
    if d == 'E':
        d = 'N'
    elif d == 'N':
        d = 'W'
    elif d == 'W':
        d = 'S'
    else:
        d = 'E'
    return d


def Turn_Right(d): # 오른쪽으로 회전
    print('Turn Right')
    if d == 'W':
        d = 'N'
    elif d == 'S':
        d = 'W'
    elif d == 'E':
        d = 'S'
    else:
        d = 'E'
    return d


def Set_State(array1, array2): # 센서 설정
    for i in range(0, 6):
        for j in range(0, 6):
            if array1[i][j] == 'B':
                array2[i][j] += 8
            elif array1[i][j] == 'G':
                array2[i][j] += 1
            elif array1[i][j] == 'W':
                array2[i - 1][j] += 2
                array2[i + 1][j] += 2
                array2[i][j - 1] += 2
                array2[i][j + 1] += 2
            elif array1[i][j] == 'P':
                array2[i - 1][j] += 4
                array2[i + 1][j] += 4
                array2[i][j - 1] += 4
                array2[i][j + 1] += 4


def Print_Wumpus_World(array): # Wumpus World 콘솔 출력
    for i in array[::-1]:
        for j in i:
            print(j, end=" ")
        print()


#GUI 생성
app = QApplication(sys.argv)
window = MyWindow()
window.show()
app.exec_()
```

