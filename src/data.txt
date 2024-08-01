import React, { useEffect, useState } from "react";
import axios from "axios";
import html2canvas from "html2canvas";
import jsPDF from "jspdf";
import "../Components/pdfDataForm.css"; // Ensure this matches your actual file name and path
import { TableBody } from "@mui/material";
// import "bootstrap/dist/css/bootstrap.css";

function PdfDataForm() {
  const handleDownloadPDF = () => {
    const input = document.getElementById("content-to-print");

    html2canvas(input, { scale: 2 }).then((canvas) => {
      const imgData = canvas.toDataURL("image/png");
      const pdf = new jsPDF("p", "mm", "a4", true);
      const imgWidth = 210; // A4 width in mm
      const pageHeight = 295;
      // A4 height in mm
      const imgHeight = (canvas.height * imgWidth) / canvas.width;
      let heightLeft = imgHeight;
      let position = 0;

      pdf.addImage(imgData, "PNG", 0, position, imgWidth, imgHeight);
      heightLeft -= pageHeight;

      while (heightLeft >= 0) {
        position -= pageHeight;
        pdf.addPage();
        pdf.addImage(imgData, "PNG", 0, position, imgWidth, imgHeight);
        heightLeft -= pageHeight;
      }

      pdf.save("download.pdf");
    });
  };

  // const projectData = {
  //   projectName: "Project Alpha",
  //   executiveSponsors: "John Doe, Jane Smith",
  //   departmentSponsor: "Marketing",
  //   umoorImpacted: "Finance, IT",
  //   projectPurpose: "To improve financial tracking and reporting.",
  //   deliverables: "Monthly financial reports, dashboard",
  //   scopeExclusions: "Does not include tax calculations.",
  //   targetAudiences: "Finance team, Executive management",
  //   projectFeasibility: "High feasibility based on initial analysis.",
  //   projectFinance: "Lorem ipsum dolor sit amet, consectetur adipiscing elit,",
  //   projectWorking:
  //     "Lorem ipsum dolor sit amet, consectetur adipiscing elit,Lorem ipsum dolor sit amet, consectetur adipiscing elit,",
  // };
  const [projectData, setProjectData] = useState({
    projectName: "",
    executiveSponsors: "",
    departmentSponsor: "",
    umoorImpacted: "",
    waqfUmoor: "",
    projectPurpose: "",
    deliverables: "",
    scopeExclusions: "",
    targetAudiences: "",
    projectFeasibility: "",
  });

  useEffect(() => {
    // Function to fetch data from API
    const fetchData = async () => {
      try {
        const response = await axios.get(
          "http://182.188.38.224:8082/waqf_test/noc_requests/fetch/getProjectCharterpg1.php?tn_id=19"
        );
        const data = response.data[0]; // Assuming the API returns an array with a single object
        const response2 = await axios.get(
          "http://182.188.38.224:8082/waqf_test/noc_requests/fetch/getProjectCharterpg2.php?tn_id=19"
        );
        const data2 = response2.data;

        setProjectData({
          projectName: data.project_name,
          executiveSponsors: data.exec_sponsors,
          departmentSponsor: data.dept_sponsor,
          umoorImpacted: data.umoor_impacted,
          waqfUmoor: data.waqf_umoor,
          projectPurpose: data.project_purpose,
          deliverables: data.deliverables,
          scopeExclusions: data.exclusions,
          targetAudiences: data.target_audiences,
          projectFeasibility: data.feasibility,
          currentWorkings: data2.crnt_workings,
          financials: data2.financials,
          projectCost: data2.project_cost,
          sowCompleted: data2.sow_completed,
          sowPending: data2.sow_pending,
        });
      } catch (error) {
        console.error("Error fetching project data:", error);
      }
    };

    fetchData();
  }, []);
  return (
    <>
      <div className="downloadPdf-section"></div>
      <div
        id="content-to-print"
        style={{ padding: "20px" }}
        className="container"
      >
        <h1 className="text-center mb-4">PROJECT CHARTER</h1>

        <table className="table table-bordered">
          <tbody>
            {/* General Project Information Section */}
            <tr className="header-row">
              <td colSpan="2">
                <h4 className="mb-3">1. General Project Information</h4>
              </td>
            </tr>
            <tr className="form-row">
              <td className="label-cell">
                <label htmlFor="projectName">Project Name:</label>
              </td>
              <td className="input-cell">
                <span>{projectData.projectName}</span>
              </td>
            </tr>
            <tr className="form-row">
              <td className="label-cell">
                <label htmlFor="executiveSponsors">Executive Sponsors:</label>
              </td>
              <td className="input-cell">
                <span>{projectData.executiveSponsors}</span>
              </td>
            </tr>
            <tr className="form-row">
              <td className="label-cell">
                <label htmlFor="departmentSponsor">Department Sponsor:</label>
              </td>
              <td className="input-cell">
                <span>{projectData.departmentSponsor}</span>
              </td>
            </tr>
            <tr className="form-row">
              <td className="label-cell">
                <label htmlFor="umoorImpacted">Umoor Impacted:</label>
              </td>
              <td className="input-cell">
                <span>{projectData.waqfUmoor}</span>
              </td>
            </tr>

            {/* Project Scope Statement Section */}
            <tr className="header-row">
              <td colSpan="2">
                <h4 className="mb-3">2. Project Scope Statement</h4>
              </td>
            </tr>
            <tr>
              <td className="label-cell" colSpan="1">
                <label htmlFor="projectPurpose" className="text-left">
                  A-Project Purpose/Justification
                </label>
              </td>
            </tr>
            <tr>
              <td>
                <span className="text-cell bigTB">
                  {projectData.projectPurpose}
                </span>
              </td>
            </tr>
            <tr>
              <td className="label-cell" colSpan="1">
                <label htmlFor="deliverables">B-Deliverables</label>
              </td>
            </tr>
            <tr>
              <td>
                <span className="text-cell smallTB">
                  {projectData.deliverables}
                </span>
              </td>
            </tr>
            <tr>
              <td className="label-cell" colSpan="1">
                <label htmlFor="scopeExclusions">
                  C-Scope Exclusions/Documents Pending
                </label>
              </td>
            </tr>
            <tr>
              <td>
                <span className="text-cell smallTB">
                  {projectData.scopeExclusions}
                </span>
              </td>
            </tr>
            <tr>
              <td className="label-cell" colSpan="1">
                <label htmlFor="targetAudiences">D-Target Audiences</label>
              </td>
            </tr>
            <tr>
              <td>
                <span className="text-cell smallTB">
                  {projectData.targetAudiences}
                </span>
              </td>
            </tr>
            <tr>
              <td className="label-cell" colSpan="1">
                <label htmlFor="projectFeasibility">
                  E-Project Feasibility
                </label>
              </td>
            </tr>
            <tr>
              <td>
                <span className="text-cell smallTB">
                  {projectData.projectFeasibility}
                </span>
              </td>
            </tr>
            {/* Project Financails Section */}
            <tr className="header-row">
              <td colSpan="2">
                <h4 className="mb-3">3. Financails</h4>
              </td>
            </tr>
            <tr>
              <td>
                <span className="text-cell smallTB">
                  {projectData.projectFinance}
                </span>
              </td>
            </tr>
            {/* Project current working Section */}
            <tr className="header-row">
              <td colSpan="2">
                <h4 className="mb-3">4. What are the current working?</h4>
              </td>
            </tr>
            <tr>
              <td>
                <span className="text-cell bigTB">
                  {projectData.projectWorking}
                </span>
              </td>
            </tr>
            {/* Project timeline & Milestones Section 5A*/}
            <tr className="header-row">
              <td colSpan="2">
                <h4 className="mb-3">
                  5A. Project Timeline & Milestones achieved (Proper start and
                  end dates for Project Phases)
                </h4>
              </td>
            </tr>
            <tr>
              <td>
                <table className="table table-bordered w-100 custom-mini-table-one">
                  <thead>
                    <tr>
                      <th colSpan="2">Scope of work</th>
                      <th>Responsibilites</th>
                      <th>Start date</th>
                      <th>End date</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr>
                      <td colSpan="2">Data 1</td>
                      <td>Data 2</td>
                      <td>Data 3</td>
                      <td>Data 4</td>
                    </tr>
                    <tr>
                      <td colSpan="2">Data 5</td>
                      <td>Data 6</td>
                      <td>Data 7</td>
                      <td>Data 8</td>
                    </tr>
                    <tr>
                      <td colSpan="2">Data 1</td>
                      <td>Data 2</td>
                      <td>Data 3</td>
                      <td>Data 4</td>
                    </tr>
                    <tr>
                      <td colSpan="2">Data 5</td>
                      <td>Data 6</td>
                      <td>Data 7</td>
                      <td>Data 8</td>
                    </tr>
                    <tr>
                      <td colSpan="2">Data 1</td>
                      <td>Data 2</td>
                      <td>Data 3</td>
                      <td>Data 4</td>
                    </tr>
                    <tr>
                      <td colSpan="2">Data 5</td>
                      <td>Data 6</td>
                      <td>Data 7</td>
                      <td>Data 8</td>
                    </tr>
                    <tr>
                      <td colSpan="2">Data 1</td>
                      <td>Data 2</td>
                      <td>Data 3</td>
                      <td>Data 4</td>
                    </tr>
                    <tr>
                      <td colSpan="2">Data 5</td>
                      <td>Data 6</td>
                      <td>Data 7</td>
                      <td>Data 8</td>
                    </tr>
                  </tbody>
                </table>
              </td>
            </tr>
            {/* Project timeline & Milestones Section 5B*/}
            <tr className="header-row">
              <td colSpan="2">
                <h4 className="mb-3">
                  5B. Project Timeline & Milestones to be achieved (Proper start
                  and end dates for Project Phases)
                </h4>
              </td>
            </tr>
            <tr>
              <td>
                <table className="table table-bordered w-100 custom-mini-table-two">
                  <thead>
                    <tr>
                      <th colSpan="2">Scope of work</th>
                      <th>Responsibilites</th>
                      <th>Start date</th>
                      <th>End date</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr>
                      <td colSpan="2">Data 1</td>
                      <td>Data 2</td>
                      <td>Data 3</td>
                      <td>Data 4</td>
                    </tr>
                    <tr>
                      <td colSpan="2">Data 5</td>
                      <td>Data 6</td>
                      <td>Data 7</td>
                      <td>Data 8</td>
                    </tr>
                    <tr>
                      <td colSpan="2">Data 1</td>
                      <td>Data 2</td>
                      <td>Data 3</td>
                      <td>Data 4</td>
                    </tr>
                    <tr>
                      <td colSpan="2">Data 5</td>
                      <td>Data 6</td>
                      <td>Data 7</td>
                      <td>Data 8</td>
                    </tr>
                  </tbody>
                </table>
              </td>
            </tr>
            {/* Project Resources Section */}
            <tr className="header-row">
              <td colSpan="2">
                <h4 className="mb-3">6. Project Resources</h4>
              </td>
            </tr>
            <tr>
              <td>
                <span className="text-cell largeTB">
                  {projectData.projectWorking}
                </span>
              </td>
            </tr>
            {/* Project stakeholder Section */}
            <tr className="header-row">
              <td colSpan="2">
                <h4 className="mb-3">7. Project stakeholder</h4>
              </td>
            </tr>
            <tr>
              <td>
                <span className="text-cell bigTB">
                  {projectData.projectWorking}
                </span>
              </td>
            </tr>
            {/* Project Major known Risk Section 5B*/}
            <tr className="header-row">
              <td colSpan="2">
                <h4 className="mb-3">
                  8. Major known Risk (including significant Assumption)
                  Identify obstacles that may cause the project to fail
                </h4>
              </td>
            </tr>
            <tr>
              <td>
                <table className="table table-bordered w-100 custom-mini-table-one">
                  <thead>
                    <tr>
                      <th colSpan={2}>Risk</th>
                      <th>Type(High,Mudium,Low)</th>
                      <th>Mitigation Plan</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr>
                      <td colSpan="2">Data 1</td>
                      <td>Data 2</td>
                      <td>Data 3</td>
                    </tr>
                    <tr>
                      <td colSpan="2">Data 5</td>
                      <td>Data 6</td>
                      <td>Data 7</td>
                    </tr>
                    <tr>
                      <td colSpan="2">Data 1</td>
                      <td>Data 2</td>
                      <td>Data 3</td>
                    </tr>
                    <tr>
                      <td colSpan="2">Data 5</td>
                      <td>Data 6</td>
                      <td>Data 7</td>
                    </tr>
                  </tbody>
                </table>
              </td>
            </tr>
            {/* Project Contraints Section */}
            <tr className="header-row">
              <td colSpan="2">
                <h4 className="mb-3">
                  9. Constraints: List any Condition that may limit the project
                  team's options with respect to resources, personnel, or
                  schedule
                </h4>
              </td>
            </tr>
            <tr>
              <td>
                <span className="text-cell smallTB">
                  {projectData.projectWorking}
                </span>
              </td>
            </tr>
            {/* Project External Dependencies Section */}
            <tr className="header-row">
              <td colSpan="2">
                <h4 className="mb-3">10. External Dependencies: </h4>
              </td>
            </tr>
            <tr>
              <td>
                <span className="text-cell bigTB">
                  {projectData.projectWorking}
                </span>
              </td>
            </tr>
            {/* Project  Communition Strategy Section */}
            <tr className="header-row">
              <td colSpan="2">
                <h4 className="mb-3">11. Communition Strategy: </h4>
              </td>
            </tr>
            <tr>
              <td>
                <table className="table table-bordered w-100 custom-mini-table-one">
                  <thead>
                    <tr>
                      <th>Owner</th>
                      <th>Audience</th>
                      <th>Goal</th>
                      <th>Frequency</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr>
                      <td>Data 1</td>
                      <td>Data 2</td>
                      <td>Data 3</td>
                      <td>Data 4</td>
                    </tr>
                    <tr>
                      <td>Data 5</td>
                      <td>Data 6</td>
                      <td>Data 7</td>
                      <td>Data 8</td>
                    </tr>
                    <tr>
                      <td>Data 1</td>
                      <td>Data 2</td>
                      <td>Data 3</td>
                      <td>Data 4</td>
                    </tr>
                    <tr>
                      <td>Data 5</td>
                      <td>Data 6</td>
                      <td>Data 7</td>
                      <td>Data 8</td>
                    </tr>
                  </tbody>
                </table>
              </td>
            </tr>
            {/* Project Tiem Period Section */}
            <tr className="header-row">
              <td colSpan="2">
                <h4 className="mb-3">12. Time Period </h4>
              </td>
            </tr>
            <tr>
              <td>
                <span className="text-cell smallTB">
                  {projectData.projectWorking}
                </span>
              </td>
            </tr>
            {/* project Team Section */}
            <tr className="header-row">
              <td colSpan="2">
                <h4 className="mb-3">13. Project Team </h4>
              </td>
            </tr>
            <tr className="form-row">
              <td className="label-cell">
                <label htmlFor="projectName">Project Manager:</label>
              </td>
              <td className="input-cell">
                <span>{projectData.projectName}</span>
              </td>
            </tr>
            <tr className="form-row bigTB">
              <td className="label-cell">
                <label htmlFor="executiveSponsors ">Team Members:</label>
              </td>
              <td className="input-cell">
                <span>{projectData.executiveSponsors}</span>
              </td>
            </tr>
            {/* Signatuers Section */}
            <tr className="header-row">
              <td colSpan="2">
                <h4 className="mb-3">14. Signatuers</h4>
              </td>
            </tr>
            <tr className="form-row">
              <td className="label-cell">
                <label htmlFor="projectName">ABC </label>
              </td>
              <td className="input-cell">
                <span>abcSignatuers</span>
              </td>
            </tr>
            <tr className="form-row">
              <td className="label-cell">
                <label htmlFor="executiveSponsors">EFG</label>
              </td>
              <td className="input-cell">
                <span>efgSignatuers</span>
              </td>
            </tr>
            <tr className="form-row">
              <td className="label-cell">
                <label htmlFor="departmentSponsor">IJK</label>
              </td>
              <td className="input-cell">
                <span>ijkSignatuers</span>
              </td>
            </tr>
            <tr className="form-row">
              <td className="label-cell">
                <label htmlFor="umoorImpacted">XYZ</label>
              </td>
              <td className="input-cell">
                <span>xyzSignatuers</span>
              </td>
            </tr>
          </tbody>
        </table>
      </div>

      <button onClick={handleDownloadPDF} className="btn btn-primary">
        Download PDF
      </button>
    </>
  );
}

export default PdfDataForm;
