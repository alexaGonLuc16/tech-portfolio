```mermaid
flowchart LR

%% Video pipeline
VS[/video_source/video_source/] --> VR[/video_source/raw/]

%% Perception nodes
VR --> CTRL[controller]
VR --> TL[traffic_light]
VR --> TS[traffic_signal]

TL --> C[/color/]
TS --> S[/signal/]

%% Controller and states
C --> G[go_to_goal]
S --> G
LS[line_state] --> G
LS[line_state] --> ODOM
POS[/position/] --> G

CTRL[controller] --> LS
CTRL --> POS
CTRL --> CMD[/cmd_vel/]

%% Motion and odometry
CMD --> PSN[puzzlebot_serial_node]
PSN --> VELR[VelocityEncR]
PSN --> VELL[VelocityEncL]

VELR --> ODOM[odometry_node]
VELL --> ODOM

ODOM --> POSE[/pose/]
ODOM --> RO[/reset_origin/]
RO --> PG
S --> PG[path_generator]

%% Path planning
PG[path_generator] --> GOAL[/goal/]
GOAL --> G

%% Odometry finish
ODOM --> OF[odometry_finish]
OF --> CTRL

%% Pose
POSE[/pose/] --> G[go_to_goal]
