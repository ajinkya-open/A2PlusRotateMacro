# A2plus Rotate Under Constraints – FreeCAD Macro

A FreeCAD macro that lets you **rotate a part around an axis (e.g., a rod)** in an A2plus assembly **while keeping all assembly constraints satisfied**.

This is especially useful for simulating revolute joints (hinges, rotating levers, etc.) where normal FreeCAD transform tools ignore assembly constraints.

## Features

- **Interactive rotation** using a slider (0–360°) or a spinbox for precise angle entry.
- **Automatic selection detection** – simply select the part and the axis (cylindrical face/edge) and the macro recognizes them.
- **Auto-rotate mode** with adjustable speed for continuous animation.
- **Reset button** to return the part to its original angle.
- **Live solver status** – displays whether the A2plus solver successfully satisfied all constraints after each rotation.
- **Non‑modal dialog** – you can still interact with the 3D view while the macro is running.
- Works even if your A2plus version lacks the built‑in rotate or angle constraint tools.

## Requirements

- FreeCAD 0.18 or newer (tested with 0.20+)
- [A2plus workbench](https://github.com/kbwbe/A2plus) installed
- An assembly with at least one part that has a free rotational degree of freedom (e.g., a part mounted on a fixed rod with axial + plane constraints)

## Installation

1. Download the macro file (`A2plus_Rotate_Under_Constraints.FCMacro`) from this repository.
2. In FreeCAD, go to **Macro → Macros → Create**, give it a name, and paste the code.
3. Save the macro. You can also add it to a custom toolbar for quick access.

## Usage

1. Open your A2plus assembly in FreeCAD.
2. Run the macro. A dialog window will appear.
3. **Select the part** you want to rotate (click on its name in the Model tree or on the part body in the 3D view). The dialog will show `Part: <name>`.
4. **Select the rotation axis** by holding `Ctrl` and clicking on the **cylindrical face** or a **circular edge** of the fixed rod (or any other axis reference). The dialog will show `Axis: selected`.
5. Use the **slider** or the **spinbox** to set the rotation angle from 0° to 360°. The part rotates around the axis, and the A2plus solver is called after each change to keep all constraints satisfied.
6. Optional: Enable **Auto Rotate** and adjust the speed slider for continuous rotation.
7. Click **Reset Angle to 0** to return to the initial position.
8. Close the dialog when done.

> **Note:** The rotation is relative to the part’s placement at the moment it was first selected. If you select a different part, the reference resets.

## How It Works

The macro performs two steps for every rotation increment:

1. **Direct rotation** – it temporarily rotates the selected part around the chosen axis.
2. **Constraint solving** – it calls the A2plus solver (`a2p_solversystem.solveConstraints`) to adjust all part placements so that every defined constraint (axial, plane, etc.) remains satisfied.

This ensures that the part stays on the rod, does not slide along it, and only moves along its allowed rotational degree of freedom.

## Limitations

- The part must have exactly **one free rotational degree of freedom** around the chosen axis. If the part is over‑constrained, the solver may fail or the part may not rotate as expected.
- The macro currently supports rotating one part at a time. For complex mechanisms with multiple coupled parts, you may need to use A2plus’s built‑in animation tools (if available).
- The rotation angle is limited to 0–360° in the UI for simplicity, but the internal logic can handle larger values (the slider wraps around).

## License

This macro is provided under the [MIT License](LICENSE). You are free to use, modify, and distribute it.

## Contributing

If you find bugs or have suggestions, please open an issue or submit a pull request.
