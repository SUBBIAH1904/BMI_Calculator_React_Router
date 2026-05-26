# Ex06 BMI Calculator
## Date: 26/05/2026
## Name : Subbiah S
## Reg no : 212223220111

## AIM
To develop a responsive and interactive Body Mass Index (BMI) Calculator using React that allows users to input their height and weight, and calculates their BMI to categorize their health status (e.g., Underweight, Normal, Overweight, Obese).

## DESIGN STEPS

### STEP 1: Initialize React Project

<li>Create a new React app using create-react-app.</li>
<li>Install React Router using:</li>
npm install react-router-dom

### STEP 2: Set Up Routing

Create routing structure with react-router-dom:

<li>Home route (/) – Intro or Navigation</li>

<li>BMI Calculator route (/bmi)</li>

<li>Result route (/result)</li>

### STEP 3: Design the BMI Form Page

<li>Create a form to accept Height (in cm or m) and Weight (in kg).</li>

<li>On form submit, navigate to the result page with entered values via URL query params or context/state.</li>

## STEP 4: Handle Input Validation

<li>Check if height and weight are valid numbers.</li>

<li>Optionally, show error messages for invalid inputs.</li>

### STEP 5: Perform BMI Calculation

<li>In the result component:

<li>Extract height and weight from the route (URL or passed state).</li>

<li>Apply the BMI formula:</li>

![image](https://github.com/user-attachments/assets/ec785506-c96b-489e-8783-fb1a5d36101a)
​
 
<li>Convert height from cm to m if needed.</li></li>

### STEP 6: Display Result

<li>Show calculated BMI.</li>

<li>Show category based on BMI range:

<li>Underweight, Normal, Overweight, Obese, etc.</li></li>

### STEP 7: Navigation Options

<li>Provide a button to go back to the BMI form to calculate again.</li>

### STEP 8: Enhancements

<li>Add styling using CSS or Tailwind.</li>

## PROGRAM
### index.html
```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Ex06 BMI Calculator</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```

### BMICalculator.jsx
```
import { ArrowLeft, Calculator } from "lucide-react";
import { useMemo, useState } from "react";
import { Link, useNavigate } from "react-router-dom";

export default function BMICalculator() {
  const navigate = useNavigate();
  const [height, setHeight] = useState("");
  const [weight, setWeight] = useState("");
  const [error, setError] = useState("");

  const previewBmi = useMemo(() => {
    const heightNumber = Number(height);
    const weightNumber = Number(weight);

    if (heightNumber <= 0 || weightNumber <= 0) {
      return "";
    }

    const heightInMeters = heightNumber / 100;
    return (weightNumber / (heightInMeters * heightInMeters)).toFixed(2);
  }, [height, weight]);

  function handleSubmit(event) {
    event.preventDefault();

    const heightNumber = Number(height);
    const weightNumber = Number(weight);

    if (!height || !weight || heightNumber <= 0 || weightNumber <= 0) {
      setError("Please enter valid positive numbers for height and weight.");
      return;
    }

    setError("");
    navigate(`/result?height=${heightNumber}&weight=${weightNumber}`);
  }

  return (
    <main className="page">
      <section className="panel form-panel">
        <Link className="back-link" to="/">
          <ArrowLeft size={18} />
          Home
        </Link>

        <p className="eyebrow">BMI Form</p>
        <h1>Enter Your Details</h1>

        <form className="bmi-form" onSubmit={handleSubmit}>
          <label>
            Height (cm)
            <input
              type="number"
              min="1"
              step="0.1"
              value={height}
              onChange={(event) => setHeight(event.target.value)}
              placeholder="Example: 170"
            />
          </label>

          <label>
            Weight (kg)
            <input
              type="number"
              min="1"
              step="0.1"
              value={weight}
              onChange={(event) => setWeight(event.target.value)}
              placeholder="Example: 65"
            />
          </label>

          {error && <p className="error-text">{error}</p>}

          <div className="live-box">
            <span>Live BMI Preview</span>
            <strong>{previewBmi || "--"}</strong>
          </div>

          <button className="primary-button" type="submit">
            <Calculator size={20} />
            Calculate BMI
          </button>
        </form>
      </section>
    </main>
  );
}
```
### styles.css
```
:root {
  color: #16211f;
  background: #f4f7f4;
  font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont,
    "Segoe UI", sans-serif;
}

* {
  box-sizing: border-box;
}

body {
  margin: 0;
  min-width: 320px;
}

a {
  color: inherit;
  text-decoration: none;
}

.page {
  align-items: center;
  background:
    linear-gradient(135deg, rgba(35, 105, 92, 0.14), transparent 38%),
    linear-gradient(315deg, rgba(231, 178, 79, 0.2), transparent 32%),
    #f4f7f4;
  display: flex;
  justify-content: center;
  min-height: 100vh;
  padding: 32px 16px;
}

.panel {
  background: rgba(255, 255, 255, 0.94);
  border: 1px solid rgba(22, 33, 31, 0.12);
  border-radius: 8px;
  box-shadow: 0 24px 70px rgba(31, 48, 44, 0.16);
  max-width: 480px;
  padding: 32px;
  width: 100%;
}

.intro-panel,
.result-panel {
  text-align: center;
}

.icon-badge {
  align-items: center;
  background: #23695c;
  border-radius: 999px;
  color: white;
  display: inline-flex;
  height: 72px;
  justify-content: center;
  margin-bottom: 18px;
  width: 72px;
}

.eyebrow {
  color: #b35c2e;
  font-size: 0.78rem;
  font-weight: 800;
  letter-spacing: 0.08em;
  margin: 0 0 8px;
  text-transform: uppercase;
}

h1,
h2,
p {
  margin-top: 0;
}

h1 {
  font-size: clamp(2rem, 7vw, 3.2rem);
  line-height: 1;
  margin-bottom: 16px;
}

.intro-text {
  color: #52605c;
  font-size: 1.05rem;
  line-height: 1.6;
  margin-bottom: 28px;
}

.primary-link,
.primary-button {
  align-items: center;
  background: #23695c;
  border: 0;
  border-radius: 8px;
  color: white;
  cursor: pointer;
  display: inline-flex;
  font: inherit;
  font-weight: 800;
  gap: 10px;
  justify-content: center;
  min-height: 48px;
  padding: 13px 20px;
  transition:
    background 160ms ease,
    transform 160ms ease;
  width: 100%;
}

.primary-link:hover,
.primary-button:hover {
  background: #194e44;
  transform: translateY(-1px);
}

.back-link {
  align-items: center;
  color: #52605c;
  display: inline-flex;
  font-weight: 700;
  gap: 8px;
  margin-bottom: 24px;
}

.bmi-form {
  display: grid;
  gap: 18px;
  margin-top: 22px;
}

label {
  color: #273632;
  display: grid;
  font-weight: 800;
  gap: 8px;
}

input {
  border: 1px solid #cbd7d3;
  border-radius: 8px;
  color: #16211f;
  font: inherit;
  min-height: 48px;
  padding: 12px 14px;
  width: 100%;
}

input:focus {
  border-color: #23695c;
  box-shadow: 0 0 0 4px rgba(35, 105, 92, 0.14);
  outline: none;
}

.error-text {
  background: #fff1ed;
  border: 1px solid #f2b7a4;
  border-radius: 8px;
  color: #9b3419;
  font-weight: 700;
  line-height: 1.5;
  margin: 0;
  padding: 12px 14px;
}

.live-box,
.category-box,
.summary-grid > div {
  background: #eef5f1;
  border: 1px solid #d9e7e0;
  border-radius: 8px;
}

.live-box {
  align-items: center;
  display: flex;
  justify-content: space-between;
  min-height: 56px;
  padding: 12px 14px;
}

.live-box span {
  color: #52605c;
  font-weight: 700;
}

.live-box strong {
  color: #23695c;
  font-size: 1.35rem;
}

.result-circle {
  align-items: center;
  background: #23695c;
  border-radius: 999px;
  color: white;
  display: inline-flex;
  flex-direction: column;
  height: 160px;
  justify-content: center;
  margin: 12px auto 22px;
  width: 160px;
}

.result-circle span {
  font-weight: 800;
  opacity: 0.82;
}

.result-circle strong {
  font-size: 2.7rem;
  line-height: 1;
}

.category-box {
  margin-bottom: 18px;
  padding: 18px;
}

.category-box p {
  color: #52605c;
  font-weight: 800;
  margin-bottom: 4px;
}

.category-box h2 {
  color: #b35c2e;
  font-size: 2rem;
  margin-bottom: 6px;
}

.category-box span {
  color: #52605c;
  line-height: 1.5;
}

.summary-grid {
  display: grid;
  gap: 12px;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  margin: 0 0 22px;
}

.summary-grid > div {
  padding: 14px;
}

dt {
  color: #52605c;
  font-weight: 800;
}

dd {
  color: #16211f;
  font-size: 1.2rem;
  font-weight: 900;
  margin: 4px 0 0;
}

@media (max-width: 520px) {
  .panel {
    padding: 24px;
  }

  .summary-grid {
    grid-template-columns: 1fr;
  }
}
```

## OUTPUT
<img width="1918" height="965" alt="image" src="https://github.com/user-attachments/assets/19332628-f7c0-4acd-bdfe-058bd91e3edc" />

<img width="1919" height="964" alt="image" src="https://github.com/user-attachments/assets/ae068413-d7ed-4dce-9fab-8c6603b2937e" />

<img width="1919" height="964" alt="image" src="https://github.com/user-attachments/assets/12c04698-717f-49b4-ba88-cc7baed0acb5" />


## RESULT
The BMI Calculator successfully takes user input for height and weight, performs the BMI calculation in real-time using React state and event handling, and displays the BMI value along with the corresponding health category.
