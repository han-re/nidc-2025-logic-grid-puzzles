# Quickstart Guide: Logic Grid Puzzle Solver

## Prerequisites

1. Node.js 20.x LTS or higher
2. Modern web browser (Chrome, Firefox, Safari, or Edge)
3. Git for version control

## Project Setup

1. Clone the repository:
   ```bash
   git clone https://github.com/han-re/nidc-2025-logic-grid-puzzles.git
   cd nidc-2025-logic-grid-puzzles
   ```

2. Install dependencies:
   ```bash
   # Backend dependencies
   cd backend
   npm install
   
   # Frontend has no dependencies to install
   cd ../frontend
   ```

3. Create required directories:
   ```bash
   mkdir -p puzzles/definitions puzzles/samples puzzles/progress
   ```

## Running the Application

1. Start the backend server:
   ```bash
   cd backend
   npm start
   # Server will start on http://localhost:3000
   ```

2. Open the frontend:
   ```bash
   cd ../frontend
   # Use your browser to open src/index.html
   # Or use a simple HTTP server:
   npx http-server src
   ```

## Development Workflow

### Running Tests

```bash
# Backend tests
cd backend
npm test               # Run all tests
npm run test:unit     # Run unit tests only
npm run test:integration  # Run integration tests
npm run test:contract    # Run API contract tests

# Frontend tests
cd frontend
npm test              # Run component tests
```

### Adding a New Puzzle

1. Create a puzzle definition JSON file in `puzzles/definitions/`:
   ```json
   {
     "id": "my-puzzle",
     "title": "My New Puzzle",
     "categories": [
       {
         "id": "category1",
         "name": "Category 1",
         "items": [
           {"id": "item1", "name": "Item 1"},
           {"id": "item2", "name": "Item 2"}
         ]
       }
     ],
     "clues": []
   }
   ```

2. Update the puzzle index:
   ```bash
   cd backend
   npm run update-index
   ```

### Making Changes

1. Create a feature branch:
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. Run tests before committing:
   ```bash
   cd backend && npm test
   cd ../frontend && npm test
   ```

3. Create a pull request following the template

## API Documentation

The backend API is available at `http://localhost:3000/api/v1`:

- `GET /puzzles` - List all available puzzles
- `GET /puzzles/{id}` - Get puzzle definition
- `GET /puzzles/{id}/state` - Get current puzzle state
- `POST /puzzles/{id}/marks` - Make a new mark
- `POST /puzzles/{id}/save` - Save current state
- `GET /puzzles/{id}/saves` - List save points

See `contracts/puzzle.yaml` for complete OpenAPI specification.

## Troubleshooting

1. **Server won't start**:
   - Check Node.js version (`node --version`)
   - Verify port 3000 is available
   - Check for error messages in terminal

2. **Changes not saving**:
   - Verify write permissions in puzzles/ directory
   - Check server logs for file system errors
   - Ensure valid puzzle ID in requests

3. **Tests failing**:
   - Run `npm run clean` to clear test cache
   - Check test data in `tests/fixtures/`
   - Verify all dependencies are installed

## Getting Help

1. Check the error logs in `backend/logs/`
2. Read the full documentation in `docs/`
3. File an issue on GitHub with:
   - Steps to reproduce
   - Expected vs actual behavior
   - Error messages and logs
   - Environment details