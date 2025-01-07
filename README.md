# reactflaskbridge

Connect React and Flask both using endpoints

# React and Flask Full Walkthrough

This guide provides a complete step-by-step process to set up a Flask backend and React frontend, connect them using endpoints, and display data dynamically.

---

## Project Structure

```
connectidea/
├── backend/                # Flask backend folder
│   ├── app.py              # Main Flask app
│   ├── requirements.txt    # Python dependencies
│   └── __init__.py         # For organizing as a Python package (optional)
│
├── frontend/               # React frontend folder
│   ├── public/             # Static assets
│   │   └── index.html      # Base HTML file
│   ├── src/                # React source files
│   │   ├── App.js          # Main React component
│   │   ├── index.js        # Entry point for React
│   │   └── api/            # Optional folder for API utilities
│   └── package.json        # Node.js dependencies
│
├── README.md               # Documentation
└── .gitignore              # Files to ignore for version control
```

---

## Backend: Flask Setup

### Step-by-Step Instructions

1. Navigate to the `connectidea` folder and create the `backend` folder:

   ```bash
   mkdir backend
   cd backend
   ```

2. Create the Flask app file:

   ```bash
   touch app.py requirements.txt __init__.py
   ```

3. Install Flask and Flask-CORS in a virtual environment:

   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   pip install flask flask-cors
   ```

4. Write the following code in `app.py`:

   ```python
   from flask import Flask, jsonify
   from flask_cors import CORS

   app = Flask(__name__)
   CORS(app)  # Enable CORS for cross-origin requests

   @app.route('/api/data', methods=['GET'])
   def get_data():
       data = {"message": "Hello from Flask!", "status": "success"}
       return jsonify(data)

   if __name__ == '__main__':
       app.run(debug=True)
   ```

5. Update the `requirements.txt` file with the following dependencies:

   ```plaintext
   Flask==3.1.0
   Flask-Cors==5.0.0
   ```

6. Run the Flask app:

   ```bash
   python app.py
   ```

   The server should now be running at `http://127.0.0.1:5000`.

### Verify Flask API:

- Open your browser and navigate to `http://127.0.0.1:5000/api/data`. You should see:
  ```json
  {"message": "Hello from Flask!", "status": "success"}
  ```

---

## Frontend: React Setup

### Step-by-Step Instructions

1. Navigate back to the `connectidea` folder and create the `frontend` folder:

   ```bash
   mkdir frontend
   cd frontend
   ```

2. Create a `package.json` file:

   ```bash
   npm init -y
   ```

3. Add the following content to `package.json`:

   ```json
   {
     "name": "frontend",
     "version": "1.0.0",
     "description": "",
     "main": "index.js",
     "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build",
       "test": "react-scripts test",
       "eject": "react-scripts eject"
     },
     "author": "",
     "license": "ISC",
     "dependencies": {
       "react": "^18.0.0",
       "react-dom": "^18.0.0",
       "react-scripts": "^5.0.1"
     },
     "browserslist": {
       "production": [
         ">0.2%",
         "not dead",
         "not op_mini all"
       ],
       "development": [
         "last 1 chrome version",
         "last 1 firefox version",
         "last 1 safari version"
       ]
     }
   }
   ```

4. Install dependencies:

   ```bash
   npm install
   ```

5. Set up the basic folder structure:

   ```bash
   mkdir public src src/api
   touch public/index.html src/App.js src/index.js
   ```

6. Write the following code in `public/index.html`:

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>React App</title>
   </head>
   <body>
       <div id="root"></div>
   </body>
   </html>
   ```

7. Write the following code in `src/App.js`:

   ```javascript
   import React, { useState } from 'react';

   function App() {
     const [messages, setMessages] = useState([]);

     const handleButtonClick = async () => {
       try {
         const response = await fetch('http://127.0.0.1:5000/api/data');
         if (!response.ok) {
           throw new Error('Network response was not ok');
         }
         const data = await response.json();
         setMessages((prevMessages) => [...prevMessages, data.message]);
       } catch (error) {
         console.error('There was a problem with the fetch operation:', error);
       }
     };

     return (
       <div>
         <button onClick={handleButtonClick}>Hi</button>
         <div>
           {messages.map((message, index) => (
             <p key={index}>Response from Flask: {message}</p>
           ))}
         </div>
       </div>
     );
   }

   export default App;
   ```

8. Write the following code in `src/index.js`:

   ```javascript
   import React from 'react';
   import ReactDOM from 'react-dom';
   import App from './App';

   ReactDOM.render(
     <React.StrictMode>
       <App />
     </React.StrictMode>,
     document.getElementById('root')
   );
   ```

9. Start the React app:

   ```bash
   npm start
   ```

### Verify React App:

- Open your browser and navigate to `http://localhost:3000`. You should see a `Hi` button.
- When you click the button, the message "Response from Flask: Hello from Flask!" will appear and dynamically append for every subsequent click.

---

## Testing Endpoints:

- **Flask API:** Visit `http://127.0.0.1:5000/api/data`.
- **React App:** Visit `http://localhost:3000`.
- Ensure Flask logs show the API being called for each button click in React.

---

## Final Notes:

- Use `flask-cors` to handle cross-origin issues when connecting React and Flask.
- Run both the Flask and React servers simultaneously for local development.
  - Flask: `python app.py`
  - React: `npm start`
- You now have a working microservice-like setup where React is the frontend and Flask serves as the backend API.

---
