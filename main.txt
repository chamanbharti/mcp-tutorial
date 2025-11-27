# simple_calculator_mcp.py
from fastmcp import FastMCP
import random
import json

# Create the MCP server
mcp = FastMCP("simple-calculator")

# Define calculator operations
@mcp.tool
def add(a: float, b: float) -> int:
    """Add two numbers together
    Args:
    a: first number
    b: second number

    Returns:
    The sum of a and b
    """
    return a + b

@mcp.tool
def random_number(min_val: int = 1, max_val: int = 100) -> int:
    """Generate a random number within a range.
    Args:
    min_val: Minimum value (default: 1)
    max_val: Maximum value (default: 100)
    """
    return random.randint(min_val,max_val)

@mcp.resource("info://server")
def server_info() -> str:
    """Get information about this server."""
    info = {
        "name": "Simple Calculator Service",
        "version": "1.0.0",
        "description": "A basic MCP server with math tools",
        "tools": ["add","random_number"],
        "author": "Chaman Bharti"
    }
    return json.dumps(info, indent=2)


if __name__ == "__main__":
    # Run the MCP server
    mcp.run(transport="http", host="0.0.0.0", port=8000)
