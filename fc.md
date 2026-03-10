flowchart TB

%% ---------- STYLE ----------
classDef input fill:#dbe7f5,stroke:#000,stroke-width:2px,color:#000;
classDef process fill:#ffffff,stroke:#000,stroke-width:2px,color:#000;
classDef decision fill:#b7c9e2,stroke:#000,stroke-width:2px,color:#000;
classDef action fill:#0b1b3a,stroke:#000,stroke-width:2px,color:#ffffff;
classDef result fill:#0b1b3a,stroke:#000,stroke-width:2px,color:#ffffff;
classDef summary fill:#dbe7f5,stroke:#000,stroke-width:2px,color:#000;

%% ---------- INPUTS ----------
A[Current Moisture<br>(Soil Sensor)]:::input
B[Weather Forecast<br>(Open-Meteo API)]:::input
C[LSTM Predictions<br>(5-Day Horizon)]:::input
D[Crop Config<br>(Threshold & Depth)]:::input

A --> E
B --> E
C --> E
D --> E

E[DAILY DECISION ENGINE<br>Loop: Day 1 → Day 5]:::action

%% ---------- DAY 1 ----------
E --> D1[Day 1]:::input
D1 --> M1[Update Moisture<br>M += rain − evap × f]:::process
M1 --> Q1{M < θcrit ?}:::decision
Q1 --> R1[Check Rain Forecast<br>P(rain) > 50%]:::process
R1 --> I1[IRRIGATE<br>12,850 L]:::action
I1 --> O1[12,850 L]:::result

%% ---------- DAY 2 ----------
E --> D2[Day 2]:::process
D2 --> M2[Update Moisture<br>M += rain − evap × f]:::process
M2 --> Q2{M < θcrit ?}:::decision
Q2 --> R2[Check Rain Forecast<br>P(rain) > 50%]:::process
R2 --> S2[SKIP<br>Rain Expected]:::process
S2 --> O2[Skip]:::process

%% ---------- DAY 3 ----------
E --> D3[Day 3]:::input
D3 --> M3[Update Moisture<br>M += rain − evap × f]:::process
M3 --> Q3{M < θcrit ?}:::decision
Q3 --> R3[Check Rain Forecast<br>P(rain) > 50%]:::process
R3 --> I3[IRRIGATE<br>9,200 L]:::action
I3 --> O3[9,200 L]:::result

%% ---------- DAY 4 ----------
E --> D4[Day 4]:::process
D4 --> M4[Update Moisture<br>M += rain − evap × f]:::process
M4 --> Q4{M < θcrit ?}:::decision
Q4 --> R4[Check Rain Forecast<br>P(rain) > 50%]:::process
R4 --> S4[SKIP<br>Rain Expected]:::process
S4 --> O4[Skip]:::process

%% ---------- DAY 5 ----------
E --> D5[Day 5]:::input
D5 --> M5[Update Moisture<br>M += rain − evap × f]:::process
M5 --> Q5{M < θcrit ?}:::decision
Q5 --> R5[Check Rain Forecast<br>P(rain) > 50%]:::process
R5 --> I5[IRRIGATE<br>7,400 L]:::action
I5 --> O5[7,400 L]:::result

%% ---------- SUMMARY ----------
O1 --> SUM
O2 --> SUM
O3 --> SUM
O4 --> SUM
O5 --> SUM

SUM[5-DAY SCHEDULE SUMMARY<br><br>
Total Water: 29,450 L<br>
Days Irrigated: 3 of 5<br>
Water Saved: ~38%<br>
Method: Drip (90%)<br>
Next Action: Day 1]:::summary
