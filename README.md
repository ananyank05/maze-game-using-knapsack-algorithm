#include <graphics.h>
#include <conio.h>
#include <stdlib.h>
#include <vector>
#include <algorithm>
#include <cmath>

struct Item {
    int value;
    int weight;
    int x, y;
};

// Function to display instructions
void instructions() {
    setcolor(12);
    settextstyle(0, HORIZ_DIR, 0);
    outtextxy(500, 280, "w -> Up");
    outtextxy(500, 290, "s -> Down");
    outtextxy(500, 300, "a -> Left");
    outtextxy(500, 310, "d -> Right");
    setcolor(WHITE);
}

// Function to draw the maze
void body() {
    setcolor(RED);
    line(0, 100, 450, 100);
    line(50, 130, 400, 130);
    line(100, 160, 450, 160);
    line(150, 190, 300, 190);
    line(100, 220, 250, 220);
    line(50, 250, 200, 250);
    line(0, 400, 500, 400);
    line(450, 370, 500, 370);
    line(0, 100, 0, 400);
    line(50, 130, 50, 370);
    line(100, 160, 100, 220);
    line(100, 280, 100, 400);
    line(150, 250, 150, 370);
    line(200, 280, 200, 400);
    line(250, 220, 250, 370);
    line(300, 190, 300, 400);
    line(350, 160, 350, 370);
    line(400, 190, 400, 400);
    line(450, 100, 450, 370);
    setcolor(14);
    circle(500, 385, 10);
    setcolor(WHITE);
}

// Function to draw the ball
void ball(int j, int k, int l) {
    setcolor(WHITE);
    circle(j, k, l);
    setfillstyle(SOLID_FILL, 14);
    floodfill(j, k, WHITE);
}

// Function to handle ball movement conditions
void condition(int *j, int *k) {
    if (*k == 110) {
        if (*j < 450 && *j > 0)
            *k += 5;
    }
    if (*j == 10) {
        if (*k < 400 && *k > 100)
            *j += 5;
    }
    if (*k == 390) {
        if (*j < 500 && *j > 0)
            *k -= 5;
    }
    if (*j == 440) {
        if (*k > 100 && *k < 380)
            *j -= 5;
    }
    if (*k == 380) {
        if (*j < 500 && *j > 440)
            *k += 5;
    }
    if (*k >= 120 && *k <= 140) {
        if (*j < 410 && *j > 40 && *k <= 130)
            *k -= 5;
        if (*j < 410 && *j > 40 && *k >= 130)
            *k += 5;
    }
    if (*k >= 150 && *k <= 170 && *j >= 90 && *j <= 450) {
        if (*k <= 160) *k -= 10;
        *k += 5;
    }
    if (*k >= 180 && *k <= 200 && *j >= 140 && *j <= 310) {
        if (*k >= 190) *k += 10;
        *k -= 5;
    }
    if (*k >= 210 && *k <= 230 && *j >= 90 && *j <= 260) {
        if (*k >= 220) *k += 10;
        *k -= 5;
    }
    if (*k <= 260 && *k >= 240 && *j <= 210 && *j > 40) {
        if (*k >= 250) *k += 10;
        *k -= 5;
    }
    if (*k <= 370 && *k >= 130 && *j <= 60 && *j >= 40) {
        if (*j <= 50) *j -= 10;
        *j += 5;
    }
    if (*k <= 220 && *k >= 160 && *j <= 110 && *j >= 90) {
        if (*j <= 100) *j -= 10;
        *j += 5;
    }
    if (*k <= 400 && *k >= 280 && *j <= 110 && *j >= 90) {
        if (*j <= 100) *j -= 10;
        *j += 5;
    }
    if (*k <= 370 && *k >= 250 && *j <= 160 && *j >= 140) {
        if (*j <= 150) *j -= 10;
        *j += 5;
    }
    if (*k <= 400 && *k >= 280 && *j <= 210 && *j >= 190) {
        if (*j <= 200) *j -= 10;
        *j += 5;
    }
    if (*k <= 370 && *k >= 220 && *j <= 260 && *j >= 240) {
        if (*j <= 250) *j -= 10;
        *j += 5;
    }
    if (*k <= 400 && *k >= 190 && *j <= 310 && *j >= 290) {
        if (*j <= 300) *j -= 10;
        *j += 5;
    }
    if (*k <= 370 && *k >= 160 && *j <= 360 && *j >= 340) {
        if (*j <= 350) *j -= 10;
        *j += 5;
    }
    if (*k <= 400 && *k >= 190 && *j <= 410 && *j >= 390) {
        if (*j <= 400) *j -= 10;
        *j += 5;
    }
}

// Function to display the game title and instructions
void caption() {
    setcolor(12);
    settextstyle(0, HORIZ_DIR, 0);
    outtextxy(10, 70, "Escape from the Maze");
    outtextxy(10, 85, "or Press 'SPACE' to EXIT");
    setcolor(WHITE);
}

// Function to draw items
void drawItems(const std::vector<Item>& items) {
    for (const auto& item : items) {
        setcolor(GREEN);
        rectangle(item.x - 5, item.y - 5, item.x + 5, item.y + 5);
        setfillstyle(SOLID_FILL, GREEN);
        floodfill(item.x, item.y, GREEN);
    }
}

// Function to check if ball collected an item
bool collectItem(int x, int y, std::vector<Item>& items, int& currentWeight, int& totalValue, int maxWeight) {
    for (auto it = items.begin(); it != items.end(); ++it) {
        if (abs(it->x - x) <= 10 && abs(it->y - y) <= 10) {
            if (currentWeight + it->weight <= maxWeight) {
                currentWeight += it->weight;
                totalValue += it->value;
                items.erase(it);
                return true;
            } else {
                setcolor(RED);
                outtextxy(10, 410, "Cannot carry more weight!");
                delay(1000);
                setcolor(BLACK);
                outtextxy(10, 410, "Cannot carry more weight!");
                return false;
            }
        }
    }
    return false;
}

// Function to solve the knapsack problem
int knapsack(int W, const std::vector<Item>& items) {
    int n = items.size();
    std::vector<std::vector<int>> K(n + 1, std::vector<int>(W + 1, 0));

    for (int i = 0; i <= n; i++) {
        for (int w = 0; w <= W; w++) {
            if (i == 0 || w == 0) {
                K[i][w] = 0;
            } else if (items[i - 1].weight <= w) {
                K[i][w] = std::max(items[i - 1].value + K[i - 1][w - items[i - 1].weight], K[i - 1][w]);
            } else {
                K[i][w] = K[i - 1][w];
            }
        }
    }
    return K[n][W];
}

// Function to manage level 1 of the game
int level_1() {
    int j = 20, k = 250, l = 10, m = 10;
    char c;
    int maxWeight = 50;
    int currentWeight = 0;
    int totalValue = 0;
    std::vector<Item> items = {
        {10, 10, 100, 140},
        {20, 15, 200, 250},
        {30, 20, 350, 190},
        {40, 25, 300, 350}
    };

    cleardevice();
    while (1) {
        setcolor(13);
        settextstyle(1, HORIZ_DIR, 6);
        outtextxy(125, 100, "MAZE Runner");
        settextstyle(0, HORIZ_DIR, 0);
        setcolor(9);
        outtextxy(0, 400, "Press 'space' to play the level");
        char ch = getch();
        if (ch == ' ') break;
    }
    cleardevice();
    body(); // draw the figure 
    ball(j, k, l); // draw the ball
    drawItems(items); // draw items
    caption();
    instructions();
    while (true) {
        condition(&j, &k); // checking condition
        if (kbhit()) {
            c = getch();
            cleardevice();
            if (c == 75 || c == 'a') j -= 5; // Left
            if (c == 80 || c == 's') k += 5; // Down
            if (c == 77 || c == 'd') j += 5; // Right
            if (c == 72 || c == 'w') k -= 5; // Up
            if (c == ' ') return 0; // Exit
        }
        collectItem(j, k, items, currentWeight, totalValue, maxWeight);
        caption();
        body(); // draw the figure
        ball(j, k, l); // draw the ball
        drawItems(items); // draw remaining items
        if (j >= 500) {
            setcolor(14);
            settextstyle(1, HORIZ_DIR, 2); // Increase font size for winner message
            outtextxy(150, 300, "You are a winner!");
            char score[50];
            sprintf(score, "Score: %d", totalValue);
            outtextxy(150, 350, score);
            
            // Solve the knapsack problem with the remaining items
            int maxKnapsackValue = knapsack(maxWeight - currentWeight, items);
            sprintf(score, "Optimal Knapsack Value: %d", maxKnapsackValue);
            outtextxy(150, 370, score);
            
            return 1;
        }
    }
    return 9999;
}

int main() {
    // Change graphics mode to full screen
    int gdriver = DETECT, gmode;
    initgraph(&gdriver, &gmode, "");

    // Check for initialization errors
    if (graphresult() != grOk) {
        printf("Graphics initialization failed!");
        return 1;
    }

    // Set the viewport for full window
    setviewport(0, 0, getmaxx(), getmaxy(), 1);
    
    int flag = level_1();
    getch();
    closegraph(); // Closes the graphics mode
    return flag;
}
