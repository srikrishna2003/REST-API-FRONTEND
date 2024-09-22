# REST-API-FRONTEND
npx create-react-app my-frontend-app
cd my-frontend-app
npm install axios react-select
import React, { useState } from "react";
import axios from "axios";
import Select from "react-select";

function App() {
  // State to manage the JSON input, response, and errors
  const [jsonInput, setJsonInput] = useState("");
  const [responseData, setResponseData] = useState(null);
  const [error, setError] = useState(null);
  const [selectedOptions, setSelectedOptions] = useState([]);

  // Options for multi-select dropdown
  const dropdownOptions = [
    { value: "alphabets", label: "Alphabets" },
    { value: "numbers", label: "Numbers" },
    { value: "highest_lowercase_alphabet", label: "Highest Lowercase Alphabet" }
  ];

  // Handler for JSON input change
  const handleInputChange = (e) => {
    setJsonInput(e.target.value);
    setError(null);  // Clear error when input changes
  };

  // Handler for form submission
  const handleSubmit = async (e) => {
    e.preventDefault();

    // Validate JSON input
    try {
      const parsedInput = JSON.parse(jsonInput);

      // Check if data is present in JSON
      if (!parsedInput.data) {
        setError("Invalid JSON input: 'data' field is required.");
        return;
      }

      // Make POST request to your backend API
      try {
        const response = await axios.post("http://your-backend-url.com/bfhl", parsedInput);
        setResponseData(response.data);  // Store the response
      } catch (err) {
        setError("Error in making API request: " + err.message);
      }
    } catch (err) {
      setError("Invalid JSON format");
    }
  };

  // Handle dropdown selection
  const handleDropdownChange = (selected) => {
    setSelectedOptions(selected);
  };

  // Render the filtered response based on the selected dropdown options
  const renderResponse = () => {
    if (!responseData) return null;

    let filteredResponse = {};
    selectedOptions.forEach(option => {
      if (responseData[option.value]) {
        filteredResponse[option.label] = responseData[option.value];
      }
    });

    return (
      <div>
        <h3>Filtered Response:</h3>
        <pre>{JSON.stringify(filteredResponse, null, 2)}</pre>
      </div>
    );
  };

  return (
    <div>
      <h1>Frontend for REST API</h1>

      {/* Form for JSON input */}
      <form onSubmit={handleSubmit}>
        <textarea
          rows="10"
          cols="50"
          placeholder='Enter valid JSON (e.g., {"data": ["A", "B", "z"]})'
          value={jsonInput}
          onChange={handleInputChange}
        />
        <br />
        <button type="submit">Submit</button>
      </form>

      {/* Display error if any */}
      {error && <div style={{ color: "red" }}>{error}</div>}

      {/* Multi-select dropdown */}
      {responseData && (
        <>
          <Select
            isMulti
            options={dropdownOptions}
            onChange={handleDropdownChange}
            placeholder="Select options to filter response"
          />
          {renderResponse()}
        </>
      )}
    </div>
  );
}

export default App;
import { useEffect } from "react";

useEffect(() => {
  document.title = "ABCD123";  // Set your roll number as title
}, []);
npm run build
