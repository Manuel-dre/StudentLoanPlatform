// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract StudentLoan {

    struct Loan {
        address student;
        uint loanamount;
        uint loanduration;
        uint interestRate;
        bool approved;
        bool disbursed;
        bool repaid;
        uint[] activityIds;
    }

    struct LoanActivity {
        uint loanId;
        string activityType;
        uint timestamp;
    }

    mapping(address => Loan) public loans;
    LoanActivity[] public loanActivities;

    event LoanApplication(address indexed student, uint loanamount, uint indexed timestamp);
    event LoanApproval(address indexed student, uint loanamount, uint indexed timestamp);
    event LoanDisbursement(address indexed student, uint loanamount, uint indexed timestamp);
    event LoanRepayment(address indexed student, uint loanamount, uint indexed timestamp);

    modifier onlyGovernment() {
        // Implementing an access control to allow only authorized government addresses
        // ...

        _;
    }

    function applyLoan(uint _loanamount, uint _loanduration, uint _interestRate) external {
        // Performing identity verification of the student
        // ...

        // Storing of loan application details
        Loan storage loan = loans[msg.sender];
        loan.student = msg.sender;
        loan.loanamount = _loanamount;
        loan.loanduration = _loanduration;
        loan.interestRate = _interestRate;
        loan.approved = false;
        loan.disbursed = false;
        loan.repaid = false;

        // Creating a loan activity for the application
        uint activityId = _createLoanActivity(msg.sender, "Loan Application");
        loan.activityIds.push(activityId);

        emit LoanApplication(msg.sender, _loanamount, block.timestamp);
    }

    function approveLoan(address _student) external onlyGovernment {
        // Performing identity verification of government bodies
        // ...

        // Loan Approval
        Loan storage loan = loans[_student];
        require(!loan.approved, "Loan has been approved");
        loan.approved = true;

        // Adding a loan activity for the loan which was just approved
        uint activityId = _createLoanActivity(_student, "Loan Approval");
        loan.activityIds.push(activityId);

        emit LoanApproval(_student, loan.loanamount, block.timestamp);
    }

    function disburseLoan(address _student) external onlyGovernment {
        // Loan disbursement 
        // ...

        // Check the loan as disbursed
        Loan storage loan = loans[_student];
        require(loan.approved, "Loan not approved");
        require(!loan.disbursed, "Loan already disbursed");
        loan.disbursed = true;

        // Adding a loan activity for our loan disbursement
        uint activityId = _createLoanActivity(_student, "Loan Disbursement");
        loan.activityIds.push(activityId);

        emit LoanDisbursement(_student, loan.loanamount, block.timestamp);
    }

    function repayLoan(address _student) external payable {
        // Loan repayment
        // ...

        // Mark the loan as being repaid
        Loan storage loan = loans[_student];
        require(loan.approved, "Loan not approved");
        require(loan.disbursed, "Loan not disbursed");
        require(!loan.repaid, "Loan already repaid");
        loan.repaid = true;

        // Adding a loan activity for the repayment
        uint activityId = _createLoanActivity(_student, "Loan Repayment");
        loan.activityIds.push(activityId);

        emit LoanRepayment(_student, msg.value, block.timestamp);
    }

    function getLoanActivityCount() external view returns(uint) {
        return loanActivities.length;
    }

    function getLoanActivity(uint _activityId) external view returns(uint, string memory, uint) {
        require(_activityId < loanActivities.length, "Invalid activity ID");

        LoanActivity storage activity = loanActivities[_activityId];
        return (activity.loanId, activity.activityType, activity.timestamp);
    }

    function getLoanActivityIds(address _student) external view returns(uint[] memory) {
        Loan storage loan = loans[_student];
        return loan.activityIds;
    }

    function _createLoanActivity(address _student, string memory _activityType) internal returns(uint) {
        uint loanId = loans[_student].activityIds.length;

        LoanActivity memory activity = LoanActivity({
            loanId: loanId,
            activityType: _activityType,
            timestamp: block.timestamp
        });

        loanActivities.push(activity);
        return loanId;
    }
}
