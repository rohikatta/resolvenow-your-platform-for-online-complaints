
## Online Complaint Registration and Management System

### Project Description

The Online Complaint Registration and Management System is a comprehensive platform that allows users to register, track, and resolve complaints efficiently. Designed to streamline the complaint-handling process, this system improves user satisfaction by providing a centralized, secure, and transparent platform for complaint management.

### Features

1.  **User Registration and Login**:
      * Users can create accounts and log in securely.
      * Email verification ensures account authenticity.
2.  **Complaint Submission**:
      * Submit detailed complaints with attachments such as images and documents.
      * Include necessary information like contact details and issue descriptions.
3.  **Tracking and Notifications**:
      * Real-time tracking of complaint status.
      * Automated email and SMS notifications for updates.
4.  **Agent Interaction**:
      * Users can communicate directly with the assigned agents.
      * Built-in messaging system for smooth communication.
5.  **Admin Management**:
      * Admins monitor, assign, and manage complaints efficiently.
      * Oversee platform operations and ensure compliance.
6.  **Security**:
      * User authentication and data encryption ensure privacy.
      * Access controls safeguard user information.

-----

### Technical Architecture

The project follows a robust client-server architecture, built on the **MERN stack** for full-stack JavaScript development, and leverages several key technologies for enhanced functionality:

  * **Frontend**: Developed with **React.js**, providing a dynamic and component-based user interface. For styling and responsive design, **Tailwind CSS** is extensively used, offering utility-first CSS for rapid UI development. API interactions are handled efficiently using **Axios**.
  * **Backend**: Powered by **Node.js** and **Express.js**, it offers secure and scalable **RESTful APIs**. **JSON Web Tokens (JWT)** are used for stateless user authentication and authorization, ensuring secure access to resources.
  * **Database**: **MongoDB** serves as the NoSQL database, ensuring scalable and efficient data storage for complaints, user information, and other system data.
  * **Real-Time Communication**: **Socket.io** enables real-time, bidirectional event-based communication between the client and server, facilitating instant notifications and live chat functionalities. **WebRTC APIs** can be integrated for future enhancements like video/audio communication.
  * **Session Management**: While JWT handles user authentication, conceptual sessions maintain user state and context across interactions, especially important for real-time functionalities and user experience.

-----

### Folder Structure

#### Backend

  * **Controllers**: Contains logic for user authentication, database connection, and complaint management.
      * `Auth.jsonwebtoken.js` (Renamed to reflect JWT usage)
      * `Connect.db.js`
      * `Login.js`
      * `Verify.email.otp.js`
      * `UserManagement.js`
  * **Models**: Defines the data schema.
      * `complaints.js`
      * `user.js`
  * **Routes**: Handles API endpoints.
      * `ComplaintRouter.js`
      * `initializeSocketHandlers.js`
      * `login.js`
  * **Utils**: Contains utility functions.
      * `email.js`
      * `otpUtils.js`
  * **Root Files**:
      * `.env`
      * `index.js`
      * `package.json`

#### Frontend

  * **Components**:
      * **Dashboard**:
          * `Admin`, `Agent`, `User` layouts and their respective components.
      * **Layouts**: Common layouts for the application.
      * **Login**: User authentication interface.
      * **Navbar**: Navigation bar for easy access.
  * **API**:
      * `AxiosInstance.jsx` (Explicitly for Axios configuration)
  * **Context**: Manages application state.
      * `AuthContext.jsx`
      * `SocketContext.jsx`
  * **Assets**: Stores static files such as images and icons.
  * **Root Files**:
      * `App.jsx`
      * `index.css`
      * `main.jsx`

-----

### Scenario: A User's Complaint Journey with Real-time Updates

Let's follow **John's** experience from the moment he lands on the website:

1.  **Registering and Verifying Identity**:

      * John navigates to the complaint system's website. He clicks on "Sign Up" and fills in his details (name, email, password).
      * Upon submission, the system sends an **OTP (One-Time Password)** to John's registered email address.
      * John enters the OTP on the website to verify his email. Once verified, his session is initiated, and he's automatically redirected to his personalized **User Dashboard**.
      * ***Behind the scenes***: The backend's `Auth.jsonwebtoken.js` handles the generation of a JWT upon successful registration/login, which is sent to the frontend. `Verify.email.otp.js` ensures the OTP verification. `email.js` and `otpUtils.js` manage OTP delivery.

2.  **Submitting a Complaint**:

      * On his dashboard, John sees a prominent "Submit New Complaint" button. He clicks it, and a form appears.
      * He provides a detailed description of his issue (e.g., "Faulty Wi-Fi Router," "Billing Discrepancy"), selects a category, and attaches supporting evidence like a photo of the router and a scanned bill.
      * He submits the complaint.
      * ***Behind the scenes***: The frontend captures this data. **AxiosInstance.jsx** is used to make a secure API call, sending the complaint data along with John's JWT for authentication, to the backend's `ComplaintRouter.js` endpoint. The `complaints.js` model stores the complaint details in MongoDB.

3.  **Real-time Notification to Admin**:

      * The moment John submits his complaint, a **real-time notification** instantly pops up on the **Admin Dashboard**. This is powered by **Socket.io**.
      * The admin sees a new entry in their complaint list, often with a "New" or "Unassigned" status, and receives an audible alert or visual cue.
      * ***Behind the scenes***: After the complaint is successfully saved, the backend's `initializeSocketHandlers.js` emits a `newComplaint` event to all connected admin clients. The Admin dashboard component, listening for this event via `SocketContext.jsx` and `AxiosInstance.jsx` to fetch details if needed, updates its UI instantly.

4.  **Admin Assigns an Agent**:

      * The admin reviews John's complaint. From their dashboard, they can see the details and available agents.
      * The admin decides to assign the complaint to "Agent Sarah," who specializes in network issues. They click "Assign Agent" and select Sarah from a dropdown.
      * ***Behind the scenes***: This action triggers a secure **Axios API call** from the Admin component to the backend's `UserManagement.js` controller, which updates the complaint's assigned agent in the database.

5.  **Agent and User Real-time Notifications for Assignment**:

      * Immediately after the admin assigns the complaint, **Agent Sarah** receives a real-time notification on her Agent Dashboard (via Socket.io) that a new complaint has been assigned to her. She also receives an email notification.
      * Simultaneously, **John** receives a real-time notification on his User Dashboard (via Socket.io) stating that his complaint has been assigned to Agent Sarah. He also gets an email update.
      * ***Behind the scenes***: The backend, after updating the complaint, emits `complaintAssigned` events to both the specific agent's connected socket and the user's connected socket, managed by `initializeSocketHandlers.js`.

6.  **Agent-User Interaction and Real-time Chat**:

      * John, seeing his complaint assigned to Sarah, decides to ask for an estimated resolution time. He uses the built-in messaging system on his dashboard to send a message to Sarah.
      * Sarah receives John's message instantly on her dashboard via the real-time chat feature. She replies, "Hi John, I'm reviewing your issue. I'll get back to you within the hour."
      * John gets Sarah's reply instantly on his end. This back-and-forth continues seamlessly.
      * ***Behind the scenes***: This is where **Socket.io** truly shines for real-time communication. Messages are exchanged directly between the user and agent sockets, facilitated by the backend. The `initializeSocketHandlers.js` manages these chat events, ensuring secure communication leveraging the established JWT for initial connection authorization.

7.  **Agent Resolves and Updates Status**:

      * After investigating John's issue, Agent Sarah resolves the problem (e.g., she remotely reboots the router and confirms the connection is stable).
      * On her dashboard, she updates the complaint status from "In Progress" to "Resolved" and adds a resolution note.
      * ***Behind the scenes***: An **Axios API call** updates the complaint status in the database via the backend.

8.  **User and Admin Real-time Notifications for Resolution**:

      * The moment Sarah changes the status to "Resolved," **John** receives an instant real-time notification on his dashboard via Socket.io that his complaint has been resolved. He also gets an email notification with the resolution details.
      * Concurrently, the **Admin Dashboard** also gets a real-time update (via Socket.io) reflecting that John's complaint is now "Resolved," providing an up-to-the-minute overview of complaint resolution rates.
      * ***Behind the scenes***: The backend emits `complaintResolved` events to the user's socket and to all admin sockets, orchestrated by `initializeSocketHandlers.js`.

9.  **Feedback**:

      * John reviews the resolution and is satisfied. He leaves a positive rating and comments on the service received from Agent Sarah.
      * ***Behind the scenes***: This feedback is sent via an **Axios API call** to the backend and stored in the database, potentially linked to the complaint and agent for performance tracking.

-----

### How to Run the Project

1.  Clone the repository:

    ```bash
    git clone <repository_url>
    ```

2.  Navigate to the project directory and install dependencies:

    ```bash
    cd ProjectCode
    npm install
    ```

3.  Start the backend server:

    ```bash
    cd backend
    node index.js
    ```

    Backend runs on `http://localhost:3000`.

4.  Start the frontend:

    ```bash
    cd frontend
    npm start
    ```

    Frontend runs on `http://localhost:5173`.

-----

### Contributors

  * **Team Size**: 5 members
  * Roles are assigned across frontend, backend, and quality assurance.

-----

### License

This project is licensed under the MIT License team VSSMS.
