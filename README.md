# Rodolfo Centeno II
## CS 499 Capstone ePortfolio
### Southern New Hampshire University

---

## Professional Self-Assessment

Coming into the Computer Science program at Southern New Hampshire University, I did not have a clear picture of where I wanted to end up. Over three years of coursework that picture got a lot clearer, and the program gave me the habit of thinking critically about the systems I was working with, not just whether they functioned but whether they were efficient, secure, and well-designed. Courses like CS 250 introduced me to the Agile software development lifecycle and the principles behind collaborative, iterative development, including how teams communicate with stakeholders and adapt to changing requirements. Learning those frameworks gave me a clear understanding of how professional software development works in team environments and how to communicate technical decisions to both technical and non-technical audiences, which is a skill that carries into every project regardless of whether you are working alone or with a team.

The three capstone artifacts in this ePortfolio tell a connected story about how I approach software. TravlrGetaways represents my ability to build and secure complex systems, with the enhancement focused on identifying a real security vulnerability and closing it through a deliberate and well-reasoned architectural change that spans the full stack. The Pirate Intelligent Agent represents my ability to reason about algorithmic trade-offs, where replacing a blunt epsilon cutoff with a gradual exponential decay showed how small algorithmic decisions can have a meaningful impact on system behavior over time. The Grazioso Salvare Dashboard rounds out the picture by showing that I think carefully about where in a system work should happen, and moving filtering logic into MongoDB aggregation pipelines was a deliberate decision that improved both efficiency and scalability while also fixing a correctness bug that had gone unnoticed in the original implementation.

Together these three artifacts, along with the network security assessment featured elsewhere on this site, reflect the kind of broad but connected technical thinking I am bringing into my career, one that sits at the intersection of software, security, and infrastructure. I am currently pursuing my CCNA certification and have been building hands-on experience in both network operations and security assessment, including vulnerability scanning, exploitation testing, and infrastructure hardening. That range reflects where I want to grow, toward roles in network engineering, security operations, or infrastructure more broadly, wherever I can keep building on the same foundation of understanding a system deeply enough to secure it. The security mindset I developed through this program, the ability to think algorithmically, and the database skills I built around efficient data handling all translate directly into that kind of work. This degree did not just teach me how to write code, it taught me how to think like an engineer, and that is something I will carry forward regardless of where my career takes me.

---

## Code Review

The code review below walks through the three artifacts selected for enhancement in this ePortfolio, covering existing functionality, areas for improvement, and planned enhancements for each category.

[Watch Code Review on Loom](https://www.loom.com/share/89c653aa21e74fecb8cb4972d3c2c520)

---

## Artifacts

---

### Category One: Software Engineering and Design
**Artifact: TravlrGetaways (CS 465: Full Stack Development I)**

[Original Code](https://github.com/rodocentcs/CS465---TravlrGetaways) | [Enhanced Code](https://github.com/rodocentcs/CS499---Enhancements/tree/main/software-engineering)

#### Narrative

TravlrGetaways is a full-stack travel booking web application I built during CS 465: Full Stack Development I in the summer of 2025. The project runs on the MEAN stack, which consists of MongoDB, Express.js, Angular, and Node.js, and includes a customer-facing frontend, an Angular-based administrator interface, and a RESTful API backend connecting both sides. The application already had a working authentication system built with Passport.js and JWT tokens, but during the code review I identified a meaningful security gap where any account created through the public register endpoint was automatically granted the same level of access as an administrator. I selected this artifact because it gave me a practical problem to solve that reflects the kind of security considerations that come up in real professional development work. The specific components that showcase my skills are the updated user model, the new requireAdmin middleware, the revised JWT generation logic, and the fixed Angular JWT interceptor.

The enhancement process involved modifying four files across the full stack. In the user model I added a boolean isAdmin field that defaults to false for every new account and updated the generateJWT method to include that flag in the token payload. In the authentication controller I made the isAdmin false assignment explicit on registration to make the security intent clear in the code. In the routes file I wrote a new requireAdmin middleware function that checks the isAdmin flag in the verified token and returns a 403 Forbidden response if it is not present, then applied it to the POST and PUT trip routes. Finally I fixed the Angular JWT interceptor, which was previously empty, so it now retrieves the token from localStorage and attaches it to the Authorization header of every outgoing API call. Authorization tests were also added to verify protection across all privileged routes.

Going through this process I learned a lot about how authentication and authorization work together as two different layers, since authenticating a user and determining what that user is allowed to do are separate concerns that need to be handled separately in the code. The biggest challenge was making sure the isAdmin flag flowed correctly through all four layers of the stack, since a gap at any one point would break the whole system. This enhancement aligns with course outcomes four and five, addressing professional web security practices through JWT-based role checking and directly mitigating a realistic exploit where an unauthorized user could register and gain admin access.

---

### Category Two: Algorithms and Data Structures
**Artifact: Pirate Intelligent Agent (CS 370: Current and Emerging Trends in Computer Science)**

[Original Code](https://github.com/rodocentcs/CS370---PirateAgent) | [Enhanced Code](https://github.com/rodocentcs/CS499---Enhancements/tree/main/algorithms)

#### Narrative

The Pirate Intelligent Agent is a deep Q-learning project I built during CS 370: Current and Emerging Trends in Computer Science in the spring of 2025. The project centers on training an AI agent to navigate an 8 by 8 maze and find treasure before a human player does. The core of the implementation is the qtrain function, which runs a reinforcement learning training loop where the agent learns through trial and error, balancing exploration and exploitation using a value called epsilon. I selected this artifact for the algorithms and data structures category because the training loop is fundamentally an algorithmic process, and the epsilon handling logic presented a clear and meaningful opportunity for improvement that directly affects how efficiently the agent learns over time.

The enhancement focused on replacing the blunt epsilon cutoff with a gradual exponential decay. In the original implementation epsilon only changed in one place, dropping sharply to 0.05 the moment the win rate crossed 0.9, which honestly is a pretty rough way to handle that transition. The enhancement adds two constants, epsilon_min set to 0.05 and decay_rate set to 0.995, and replaces the cutoff with a formula that runs after every epoch: epsilon equals the maximum of epsilon_min and epsilon times decay_rate. The result is that epsilon now decreases smoothly from its starting value of 0.1 down toward 0.05 over the course of training, giving the agent a much more natural transition from exploration to exploitation as it gradually builds confidence in its learned policy. Epsilon tracking was also added to quantify the impact of the decay over the full training run.

Going through this process I got a much better feel for how small algorithmic decisions can have a real impact on how a system behaves over time. Working through why a gradual decay produces smoother learning behavior than a sudden drop made the trade-off reasoning behind the change a lot clearer. The main challenge was making sure the decay constants were defined in the right place in the notebook so they were accessible within the qtrain function without disrupting any of the existing code. This enhancement aligns with course outcomes three and four, addressing deliberate trade-off reasoning through the exponential decay choice and reflecting industry-standard machine learning practices through its implementation.

---

### Category Three: Databases
**Artifact: Grazioso Salvare Dashboard (CS 340: Client/Server Development)**

[Original Code](https://github.com/rodocentcs/CS-340---DashboardandREADME) | [Enhanced Code](https://github.com/rodocentcs/CS499---Enhancements/tree/main/databases)

#### Narrative

The Grazioso Salvare Dashboard is a data-driven web application I built during CS 340: Client/Server Development in the spring of 2025. The project consists of two main files, an AnimalShelter class that handles all CRUD operations against a MongoDB database of animal shelter records, and a Dash notebook that builds an interactive dashboard on top of it. The dashboard lets users filter animals by rescue type using radio buttons, with results updating dynamically in a data table, a pie chart, and a geolocation map. I selected this artifact for the databases category because the core of the project is the relationship between the application and the database, and there were some real opportunities to improve both the correctness and the efficiency of that relationship.

The enhancement involved two changes. The first was fixing a bug in the update method of the AnimalShelter class where a missing return statement caused even valid updates to raise a ValueError, which basically made the method broken regardless of the input. The second and more substantial change was replacing the pandas-based filtering in the dashboard with a MongoDB aggregation pipeline. In the original version, every filter selection pulled all matching records from the database and then filtered them in Python, which is pretty inefficient when you think about it. The new getAnimalsByRescueType method pushes the match and sort logic directly to the database layer, reducing the amount of data transferred over the network and making the whole thing more scalable as the dataset grows.

Going through this process I got a much better feel for the difference between filtering at the application layer versus the database layer, and why that distinction actually matters at scale. The bug fix was straightforward once I spotted it, but tracing why valid updates were always raising errors was a good reminder of how a single missing line can completely break expected behavior. The bigger challenge was restructuring the dashboard callback to work with the aggregation pipeline results rather than a pandas dataframe, since the data coming back needed to be handled a little differently. This enhancement aligns with course outcomes three and four, addressing the architectural trade-off reasoning behind moving filtering to the database layer and demonstrating fluency with MongoDB aggregation pipelines as a professional-grade database tool.

---

## Additional Project: Network Security

### Network Vulnerability Assessment (IT 320: Network Security)

[Full Report PDF](https://rodocentcs.github.io/IT%20320_%207-1%20FINAL%20PROJECT%20-%20CENTENO.pdf)

#### Narrative

This project was a full network security assessment completed for IT 320: Network Security in the spring of 2026. Working within a simulated enterprise environment, I used Nmap and Zenmap to identify eleven unnecessary inbound services exposed on a pfSense firewall, then used OpenVAS and Metasploit to determine which underlying vulnerabilities could actually be exploited. Wireshark and NetworkMiner were used to analyze captured network traffic, which confirmed that login credentials and file transfers were being sent in cleartext across multiple services.

Beyond the firewall, the assessment identified an internal Windows Server that was missing a critical patch for a publicly known SMBv2 vulnerability and had no antivirus or endpoint protection installed. A backdoor file placed on the server during testing had gone completely undetected until endpoint protection was deployed and a scan was run. A separate host was also found running a database service with unchanged default credentials, which was successfully exploited to gain a foothold on the machine.

I selected this project because it moved beyond identifying problems and into actually fixing them. I hardened the pfSense firewall by removing all unnecessary NAT forwarding rules and restricting inbound access to only HTTPS, RDP, and SSH, disabled the unnecessary services running on the Windows Server, and deployed antivirus software that detected and quarantined the backdoor. Every remediation was verified through follow-up scans confirming the exposure had actually been closed, not just addressed on paper. The project also required prioritizing four separate vulnerabilities by risk and likelihood and building a clear rationale for why the firewall exposure and social engineering risk needed to be addressed before the server-level issues, which sharpened how I think about triaging real security problems rather than fixing whatever is easiest first.

---

*Rodolfo Centeno II | CS 499 Capstone | Southern New Hampshire University*
