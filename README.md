# iOS App for Visualizing Network Simulation and Cyber Operations Research Gym (CybORG) Interaction

## Motivation

## Quick Start

### Setup Instructions
1. Download Xcode
2. Clone the repository: `git clone https://github.com/justinyeh1995/CybORG-ARViz.git`
 
### Running the App
To run the app on your device, follow these steps:

1. Open the project in Xcode.
2. Connect your iOS device(I'm using iPad 10th gen) via USB or USBC.
3. Select your device from the target dropdown in Xcode.
4. Click Run to build and deploy the application to your device.
   
![326292281-fab07077-90c8-454d-924a-defe0b03a34c 2](https://github.com/justinyeh1995/CybORG-ARViz/assets/42970023/a45e956f-cf2c-4b92-9948-e0802f0c2914)


## App Procedure

1. Press the start ▶ button to initialize a new game. If the game is successfully initialized, an ID will be displayed.
2. Click the next ⏭️ button to start the first step of the game.
3. Once you reach round 2, you can press the previous step ⏮️ button to view historical data.
4. Rotation of the network is supported. You can rotate the network to view it from different angles.
5. Translation of the network within the scene is supported. You can move the network around the scene. 
6. Scaling of the network within the scene is supported. You can zoom in or out to adjust the size of the network.
7. Press the end ⏹️ button to end the current game.

## Video Demonstration

### Full Video 🎥
- Youtube: https://youtu.be/MSxnLXjdzDI

#### Rotate - Translate - Scale
  
https://github.com/justinyeh1995/CybORG-ARViz/assets/42970023/2b8e19d8-1ccc-44fe-86a1-a7e5f4a06bb3

## Limitations

1. User sessions: Please note that user sessions are not currently supported. This means that the app does not store or track individual user data or progress.
2. Interaction with 3D characters and nodes: At the moment, there is no interaction available with the 3D characters and nodes within the app. Users can view the network and its components, but cannot directly interact with individual elements.

## Project Description

### 1. Objective:
This project seeks to innovate in the field of cyber operations research by employing ARKit to develop an immersive tool for visualizing the results of the Cyber Operations Research Gym (CybORG) games on iOS devices. Acknowledging the challenge of presenting the complex, dynamic outcomes of CybORG simulations on conventional displays, our initiative aims to leverage augmented reality's spatial and interactive capabilities. This approach will transform how users comprehend and analyze CybORG game outcomes, making detailed cyber operations data more accessible, engaging, and understandable.

### 2. Technology Stack:
AR Technology: Utilizing ARKit, with a focus on its advanced world tracking, plane detection, and spatial anchoring features, to create stable and interactive AR experiences tailored for visualizing CybORG game data.
Development Environment: Xcode
Data Integration: Implementing REST API calls to fetch real-time or historical CybORG game results, ensuring the visualization accurately reflects the nuanced dynamics of cyber operations simulations.

Technical Implementation in ARKit:
- Swift & SwitUI for iOS dev
- RealityKit for 3D Models rendering.
- ARKit Plane Detection: Use ARKit’s plane detection to place the network simulation on a flat surface, like a table or floor, allowing users to view it from a comfortable perspective.
- Touch Gestures: Implement gesture recognition to allow users to interact with the network.
 - Simple gestures like a tap could reveal more information, and pinch or swipe gestures could navigate through simulation steps or scale the visualization.
   

Backend API Service Code can be found at: \
https://github.com/justinyeh1995/CybORG-live/blob/main/src/api/v1/CybORG/CybORG/FastAPI/fastapi_index.py

### 3. Project Scope:
1. CybORG Data Visualization: Creating an intuitive AR interface that allows users to navigate and interact with 3D representations of CybORG game results, highlighting key metrics, attack vectors, and defense mechanisms within a cyber operations scenario.
2. Adaptive Display for Detailed Analysis: The application will enable users to scale and manipulate the spatial visualization of game results, accommodating the need for detailed analysis without being constrained by the screen size limitations of iPhones and iPads.


### 4. Class Diagrams for Important Components

`ContentView.swift`

```mermaid
graph TD
    A[ContentView] --> B{Game State}
    B --> C[No Game Started]
    B --> D[Game in Progress]
    D --> E[Loading]
    D --> F[Game ID and Round Information]
    D --> G[Observation Info View]
    A --> H[ARViewContainer]
    A --> I[Control Buttons]
    I --> J[Start Simulation]
    I --> K[Previous Step]
    I --> L[Next Step]
    I --> M[End Simulation]
    J --> N[fetchStartGame]
    N --> O{API Call}
    O -- Success --> P[Update Game ID]
    O -- Failure --> Q[Handle Error]
    L --> R[fetchNextGraphData or fetchHistoryGraphData]
    R --> S{API Call}
    S -- Success --> T[Update Graph Data and Current Step]
    S -- Failure --> U[Handle Error]
    K --> V[fetchHistoryGraphData]
    V --> W{API Call}
    W -- Success --> X[Update Graph Data and Current Step]
    W -- Failure --> Y[Handle Error]
    M --> Z[fetchEndGame]
    Z --> AA{API Call}
    AA -- Success --> AB[Reset Game State]
    AA -- Failure --> AC[Handle Error]
```

`ARViewContainer.swift`

```mermaid
graph TD
    A[ARViewContainer] --> B{Input Parameters}
    B --> C[graphData]
    B --> D[maxSteps]
    B --> E[redAgent]
    B --> F[blueAgent]
    A --> G[makeUIView]
    G --> H[Create ARView]
    G --> I[Configure ARWorldTrackingConfiguration]
    G --> J[Call updateContent]
    A --> K[updateUIView]
    K --> L[Call updateContent]
    A --> M[updateContent]
    M --> N[Remove existing anchors]
    M --> O[Create rootAnchor and parentModelEntity]
    M --> P{Check graphData}
    P --> |Empty| Q[Print empty graphData]
    P --> |Not Empty| R[Call createNodes]
    P --> |Not Empty| S[Call createLinks]
    P --> |Not Empty| T[Add labels and configure parentModelEntity]
    P --> |Not Empty| U[Add parentModelEntity to rootAnchor]
    P --> |Not Empty| V[Add rootAnchor to ARView scene]
    P --> |Not Empty| W[Install gestures on parentModelEntity]
    A --> X[createNodes]
    X --> Y[Iterate over graph nodes]
    Y --> Z[Create sphere ModelEntity for each node]
    Y --> AA[Configure node properties and add to parentModelEntity]
    A --> AB[createLinks]
    AB --> AC[Iterate over graph links]
    AC --> AD[Find source and target nodes]
    AD --> AE[Create line ModelEntity for each link]
    AD --> AF[Configure line properties and add to parentModelEntity]
    A --> AG[Extension Methods]
    AG --> AH[Entity extensions]
    AH --> AI[createTextEntity]
    AH --> AJ[addTextLabel]
    AH --> AK[createModelEntity]
    AH --> AL[addCharacter]
    AG --> AM[UIColor extension]
    AM --> AN[from colorName]
```

`GraphWrapper.swift`
```mermaid
classDiagram
    class GraphWrapper {
        +GraphDetails Blue
        +GraphDetails Red
    }
    
    class GraphDetails {
        +NetworkGraph link_diagram
        +NodePosition[] node_positions
        +String[] node_colors
        +Border[] node_borders
        +String[] compromised_hosts
        +String[] host_info
        +ActionInfo action_info
        +Map<String, String> host_map
    }
    
    class NetworkGraph {
        +Node[] nodes
        +Link[] links
    }
    
    class Node {
        +String id
    }
    
    class Link {
        +String source
        +String target
    }
    
    class NodePosition {
        +String id
        +Float x
        +Float y
        +Float z
    }
    
    class ActionInfo {
        +String action
        +Bool success
    }
    
    class Border {
        +Int width
        +String color
    }
    
    GraphWrapper *-- GraphDetails
    GraphDetails *-- NetworkGraph
    GraphDetails *-- NodePosition
    GraphDetails *-- Border
    GraphDetails *-- ActionInfo
    NetworkGraph *-- Node
    NetworkGraph *-- Link
```

## Usage

## Contributing

## Referneces

Generating 3D objects
- https://sketchfab.com/

SwiftUI
- [2023 | CS193p - Developing Apps for iOS](https://cs193p.sites.stanford.edu/2023)
- https://developer.apple.com/tutorials/swiftui
- https://www.hackingwithswift.com/books/ios-swiftui/
- https://www.youtube.com/@SwiftfulThinking
- https://www.swiftful-thinking.com/blog

ARKit
- https://developer.apple.com/documentation/arkit/arkit_in_ios

USDZ 
- https://developer.apple.com/augmented-reality/quick-look/

RealityKit
- https://divalue.github.io/Awesome-RealityKit/
- https://developer.apple.com/documentation/realitykit/
- https://www.youtube.com/watch?v=nBZ-dglGow0&list=PLBv1NzmBcY51I2qFurJv8Kk5jw1JIJeeY
- https://www.youtube.com/watch?v=qGksl5tLE-8
- https://www.youtube.com/watch?v=9R_G0EI-UoI
- https://betterprogramming.pub/how-to-add-text-to-an-arview-in-an-ios-application-tutorial-f3f746f4dc1f
- https://www.ralfebert.com/ios/realitykit-dice-tutorial/

Handling gestures
- https://www.hackingwithswift.com/articles/71/how-to-use-the-coordinator-pattern-in-ios-apps
- https://www.youtube.com/watch?v=g4UHwTqnDIY&t=401s
- https://www.createwithswift.com/using-realitykit-gestures-in-an-ar-application-with-swiftui/#:~:text=Gestures%20for%20rotation%2C%20scaling%20and,supported%20gestures%20installed%20on%20it
