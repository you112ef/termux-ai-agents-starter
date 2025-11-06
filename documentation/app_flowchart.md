flowchart TD
    Start[Start]
    Start --> Home[Landing Page]
    Home --> AuthCheck{Authenticated?}
    AuthCheck -->|No| Login[Login Page]
    AuthCheck -->|Yes| Dashboard[Dashboard]
    Login --> AuthProcess{Valid credentials?}
    AuthProcess -->|No| LoginError[Show error on login page]
    AuthProcess -->|Yes| Dashboard
    Dashboard --> Settings[Settings Page]
    Dashboard --> Visualization[Data Visualization]
    Dashboard --> Management[Data Management]
    Dashboard --> Logout[Logout]
    Logout --> Home