# password-auditor
A client-side Privacy-First Password Auditor with a Matrix/Hacker UI. Calculates Information Entropy and checks against breached databases using k-Anonymity (SHA-1) without ever sending your password to a server


%%{init: {'theme': 'dark', 'themeVariables': { 'primaryColor': '#00ff00', 'edgeLabelBackground':'#0d1117', 'tertiaryColor': '#1e1e1e'}}}%%
sequenceDiagram
    autonumber
    actor User
    participant Terminal as 💻 Local Terminal (auditor.py)
    participant API as ☁️ HIBP API (Remote)

    rect rgb(20, 20, 20)
    note right of User: 🛡️ PRIVACY SAFE ZONE (Data stays here)

    User->>Terminal: 1. Input password ("mypassword")
    activate Terminal
    note right of Terminal: 🔒 2. Hash locally (SHA-1)<br/>Full hash: 91DFD...AD4F

    Terminal->>Terminal: 3. Split Hash (k-Anonymity)
    note right of Terminal: Isolate prefix (91DFD).<br/>Keep suffix private.
    end

    rect rgb(40, 20, 20)
    note right of API: ⚠️ PUBLIC INTERNET
    Terminal->>API: 4. Send PREFIX ONLY ("91DFD")
    activate API
    note left of API: API doesn't know full hash.<br/>Finds all matches starting with 91DFD.

    API-->>Terminal: Return list of potential matches
    deactivate API
    end

    rect rgb(20, 20, 20)
    note right of User: 🛡️ PRIVACY SAFE ZONE
    Terminal->>Terminal: 5. Local Verification
    note right of Terminal: Compare private suffix against downloaded list.

    Terminal-->>User: 6. Display Result & Donation Link
    deactivate Terminal
    end
