# GROMACS-simulation-visuals
import pandas as pd
import matplotlib.pyplot as plt

# Set file paths directly to files on the Desktop if saved there 
rmsd_path = "C:/Users/DELL/Desktop/rmsd.xvg"  # Replace "DELL" with your actual username if different
gyrate_path = "C:/Users/DELL/Desktop/gyrtae.xvg"  # Replace "DELL" with your actual username if different

# Function to load .xvg data file
def load_xvg(file_path):
    """Reads an .xvg file, skipping lines that start with '@' or '#'."""
    return pd.read_csv(file_path, comment='@', sep=r'\s+', header=None, names=["Time (ps)", "Value"], on_bad_lines='skip')

# Load the RMSD and Gyration Radius data
try:
    rmsd_data = load_xvg(rmsd_path)
    gyrate_data = load_xvg(gyrate_path)

    # Debugging: Check the first few rows of the data
    print("RMSD Data:")
    print(rmsd_data.head())
    print("\nGyrate Data:")
    print(gyrate_data.head())

    # Ensure columns are numeric
    rmsd_data["Time (ps)"] = pd.to_numeric(rmsd_data["Time (ps)"], errors='coerce')
    rmsd_data["Value"] = pd.to_numeric(rmsd_data["Value"], errors='coerce')

    gyrate_data["Time (ps)"] = pd.to_numeric(gyrate_data["Time (ps)"], errors='coerce')
    gyrate_data["Value"] = pd.to_numeric(gyrate_data["Value"], errors='coerce')

    # Drop rows with NaN values (if any)
    rmsd_data = rmsd_data.dropna()
    gyrate_data = gyrate_data.dropna()

    # Plot RMSD data
    plt.figure(figsize=(10, 5))
    plt.plot(rmsd_data["Time (ps)"], rmsd_data["Value"], label="RMSD", color='blue')
    plt.xlabel("Time (ps)")
    plt.ylabel("RMSD (nm)")
    plt.title("RMSD over Time")
    plt.legend()
    plt.grid()
    plt.show()

    # Plot Gyration Radius data
    plt.figure(figsize=(10, 5))
    plt.plot(gyrate_data["Time (ps)"], gyrate_data["Value"], label="Gyration Radius", color='green')
    plt.xlabel("Time (ps)")
    plt.ylabel("Gyration Radius (nm)")
    plt.title("Radius of Gyration over Time")
    plt.legend()
    plt.grid()
    plt.show()

except FileNotFoundError:
    print("One of the file paths is incorrect. Please check that both files are on your Desktop and paths are correct.")
