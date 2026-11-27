![Build](https://github.com/Azure/bicep-types-az/workflows/Build/badge.svg) [![codecov](https://codecov.io/gh/Azure/bicep-types-az/branch/main/graph/badge.svg)](https://codecov.io/gh/Azure/bicep-types-az) [![Build Status](https://dev.azure.com/msazure/One/_apis/build/status/OneBranch/BicepMirror-Types-Az/BicepMirror-Types-Az-Official?repoName=BicepMirror-Types-Az&branchName=main)](https://dev.azure.com/msazure/One/_build/latest?definitionId=179851&repoName=BicepMirror-Types-Az&branchName=main)

# bicep-types-az
Bicep type definitions for ARM resources

## Browsing available types
See [/generated/index.md](./generated/index.md) for a searchable list of all the available types. You can view individual type definitions by clicking on api versions.

## Re-generating types from swagger
1. Under [GitHub Actions](https://github.com/azure/bicep-types-az/actions), run the "Update Types" workflow on the main branch. Usually you'll want to leave the git ref input as "master" for this workflow.
1. After ~2hrs, this action will generate a PR in this repo from a branch named `autogenerate`.
1. To workaround [this issue](https://github.com/peter-evans/create-pull-request/issues/48), close the PR and open it to trigger Merge Validation to run.
1. Merge the PR. Type differences can be reviewed by looking at the Markdown files in [/generated](./generated).

## Running generation locally
### Initial setup
1. Clone this repo
1. Check out submodules:
    ```sh
    git submodule update --init --recursive
    ```
1. Build the `bicep-types` dependency:
    ```sh
    pushd bicep-types/src/bicep-types
    npm ci
    npm run build
    popd
    ```
1. Ensure you have a copy of the [azure-rest-api-specs](https://github.com/Azure/azure-rest-api-specs) repo checked out locally.
1. Build the autorest extension code:
    ```sh
    cd src/autorest.bicep
    npm ci
    npm run build
    ```
1. Change to the generator directory, and install dependencies:
    ```sh
    cd ../../src/generator
    npm ci
    ```

### Running
1. To run generation across the entire specs repo:
    ```sh
    npm run generate -- --specs-dir {path to azure-rest-api-specs}
    ```
1. To run generation for a single path in the specs repo (e.g. 'compute'):
    ```sh
    npm run generate -- --specs-dir {path to azure-rest-api-specs} --single-path compute
    ```
1. To see other available generation parameters, including debugging options:
    ```sh
    npm run generate -- --help
    ```

### Debugging in VSCode
1. Ensure you have cloned the [azure-rest-api-specs](https://github.com/Azure/azure-rest-api-specs) repo into the same directory that you have cloned this repo - e.g. so you have the following folder structure:
    ```
    .
    ./bicep-types-az
    ./azure-rest-api-specs
    ```
1. Use the `Generate Single` VSCode action, and specify a 'base path' from the `specifications` folder in the [azure-rest-api-specs](https://github.com/Azure/azure-rest-api-specs) repo - e.g. `compute` or `automation`. You should now be able to step through the `src/generator` and `src/autorest.bicep` extension code with the VSCode debugger.

## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
https://cla.opensource.microsoft.comhttps://opensource.microsoft.com/codeofconduct/https://opensource.microsoft.com/codeofconduct/faq/opencode@microsoft.comezkl --help
ezkl <command> --help  }

  if (event.type === 'payout.failed' || event.type === 'payout.canceled') {
    // Works from released OR provisional. Never touches a settled batch.
    const b = await db.query(
      `update batches set status='open', payout_id=null, window_end=null
       where id=$1 and status in ('released','provisional')
       returning currency, total`,
      [batchId]
    );
    if (b.rowCount === 1) {
      await db.query(
        `insert into ledger (wallet, batch_id, type, amount) values ($1,$2,'reversal',$3)`,
        [b.rows[0].currency, batchId, b.rows[0].total]
      );
    } else {
      console.error(`Payout ${p.id} failed but batch ${batchId} is already settled. Manual review needed.`);
    }
  }
}

// Run daily (cron): close the failure window on provisional batches.
async function settleProvisionalBatches() {
  const r = await pool.query(
    `update batches set status='settled' where status='provisional' and window_end <= current_date returning id`
  );
  return r.rows.map((x) => x.id);
}

function addBusinessDays(from, n) {
  const d = new Date(from);
  while (n > 0) {
    d.setUTCDate(d.getUTCDate() + 1);
    if (d.getUTCDay() % 6 !== 0) n--; // skips Sat and Sun; add a holiday list for exact banking days
  }
  return d.toISOString().slice(0, 10);
}

app.listen(process.env.PORT || 3000);
module.exports = { settleProvisionalBatches };
event.typex.idp.idmodule.exportsSummarize it: a short version covering payout schedules, settlement timing, and what to expect for your first payout.
Answer a specific question: for example, how long a payout will take, what bank details a US account needs, or why a payout might fail.
Set something up: such as walking through adding a bank account, choosing a payout schedule, or testing payouts with the test account numbers.
Build something: like a checklist, a code snippet using the payouts API, or a comparison of manual vs. automatic payouts.Ready for step 2, the webhook handler (Node, with signature verification, idempotent event handling and the paid, failed and negative-payout cases)? Say "next" and I'll build it.