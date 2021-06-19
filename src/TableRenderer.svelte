<script lang="ts">
    import SvelteTable from 'svelte-table'

    var rows = [];
    var columns = [];

    export function setArray(json : any[])
    {
        rows = json;
        let headerRow = rows.splice(0, 1);
        columns = headerRow[0].map((ele : string, idx)=>{
            return {
                key: ele.toLocaleLowerCase()
                    .split(" ")
                    .join(""),
                title: ele,
                value: v => v[idx]
            }
        });
    }
</script>

<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/5.0.0-alpha1/css/bootstrap.min.css" integrity="sha384-r4NyP46KrjDleawBgD5tp8Y7UzmLA05oM1iAEQ17CSuDqnUK2+k9luXQOfXJCJ4I" crossorigin="anonymous">
<div class="container">
    <div class="row">
        <SvelteTable columns="{columns}" 
                rows="{rows}"
                classNameTable={['table table-striped']}
                classNameThead={['table-primary']}
                classNameSelect={['custom-select']} >
        </SvelteTable>
    </div>
</div>
