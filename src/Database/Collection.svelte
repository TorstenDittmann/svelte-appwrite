<script>
  import Appwrite from "../appwrite";

  export let collection;
  export let filters = [];
  export let offset = 0;
  export let limit = 50;
  export let orderField = "";
  export let orderType = "";
  export let orderCast = "string";
  export let search = "";
  export let first = 0;
  export let last = 0;

  const fetchDocuments = () =>
    Appwrite.sdk.database.listDocuments(
      collection,
      filters,
      offset,
      limit,
      orderField,
      orderType,
      orderCast,
      search,
      first,
      last
    );

  const actions = {
    reload: () => {
      documents = fetchDocuments();
    },
    create: async (data, read = ["*"], write = ["*"]) => {
      await Appwrite.sdk.database.createDocument(collection, data, read, write);
      actions.reload();
    },
  };

  let documents = fetchDocuments();
</script>

{#await documents}
  <slot name="loading" />
{:then current}
  <slot documents={current.documents} {actions} />
{:catch error}
  <slot name="error" {error} />
{/await}
