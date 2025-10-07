
# Inside the project folder
# Remove old venv if present
rm -rf venv

# Create a new one
python3 -m venv venv

# Activate it
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt