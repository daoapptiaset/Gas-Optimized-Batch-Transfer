# Gas-Optimized-Batch-Transfer
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract BatchTransfer {
    error LengthMismatch();
    error TransferFailed();

    function batchSendETH(address[] calldata recipients, uint256[] calldata amounts) public payable {
        if (recipients.length != amounts.length) revert LengthMismatch();

        uint256 total;
        for (uint i = 0; i < amounts.length; i++) {
            total += amounts[i];
        }
        require(msg.value == total, "Incorrect ETH sent");

        for (uint i = 0; i < recipients.length; i++) {
            (bool success, ) = recipients[i].call{value: amounts[i]}("");
            if (!success) revert TransferFailed();
        }
    }
}
