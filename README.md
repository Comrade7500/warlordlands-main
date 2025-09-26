# Warlordlands Game Service

A Node.js/Express web service for managing the Warlordlands game database.

CC BY-NC-SA 4.0

https://creativecommons.org/licenses/by-nc-sa/4.0/

## Prerequisites

- Node.js (v14 or higher)
- MariaDB/MySQL database
- Warlordlands database setup (run the SQL scripts first)

## Installation

1. **Install dependencies**:
   ```bash
   npm install
   ```



2. **Set up environment variables**:
   Create a `.env` file in the root directory:
   ```
   # Database Configuration
   DB_HOST=localhost
   DB_USER=warlordlands_user
   DB_PASSWORD=your_password
   DB_NAME=warlordlands
   
   # Server Configuration
   PORT=3000
   SESSION_SECRET=your-super-secret-session-key-change-this-in-production
   
   # Security (set to true in production with HTTPS)
   NODE_ENV=development
   ```

3. **Set up the database**:
   ```bash
   # Run the database creation script
   mysql -u root -p < dbscripts/001_create_database.sql
   
   # Run the sample data script
   mysql -u root -p < dbscripts/100_sample_data.sql
   ```

4. **Create admin user password hash**:
   ```bash
   # Generate a password hash (replace 'your_password' with actual password)
   node -e "const bcrypt = require('bcrypt'); bcrypt.hash('your_password', 10).then(hash => console.log(hash));"
   ```
   
   Update the sample data script with the generated hash:
   ```sql
   UPDATE users SET password_hash = 'generated_hash_here' WHERE nick = 'admin';
   ```

## Usage

1. **Start the servers**:

   # Admin server
   npm run dev_admin   # Käynnistää admin-palvelimen kehitystilassa (automaattinen uudelleenkäynnistys, näkee virheet konsolissa)
   npm start admin     # Käynnistää admin-palvelimen tuotantotilassa

   # Game server
   npm run dev game    # Käynnistää pelipalvelimen kehitystilassa (automaattinen uudelleenkäynnistys)
   npm start game      # Käynnistää pelipalvelimen tuotantotilassa

2. **Access the admin panel**:
   Open your browser and go to `http://localhost:3030`

2. **Game/user panel**:
   Open your browser and go to `http://localhost:3000`   

3. **Login**:
   Admin:
   - Username: `admin`
   - Password: `your_password` (or whatever you set)
   User:
   - Email: `player1@example.com.invalid`
   - Password: `player123`

## Security Notes

- Change the default password in production
- Use HTTPS in production environments
- Update the session secret
- Consider implementing rate limiting
- Add input validation for production use

## Development

- **Auto-restart**: Use `npm run dev` for development
- **Logs**: Check console output for errors
- **Database**: Ensure MariaDB is running and accessible

## File Structure

```
├── server.js              # Main server file
├── package.json           # Dependencies and scripts
├── .env                   # Environment configuration
├── views/                 # EJS templates
│   ├── layout.ejs        # Base layout
│   ├── login.ejs         # Login page
│   ├── admin.ejs         # Dashboard
│   ├── table.ejs         # Table listing
│   ├── edit.ejs          # Edit form
│   └── add.ejs           # Add form
└── dbscripts/            # Database scripts
    ├── 001_create_database.sql
    └── 100_sample_data.sql
```
