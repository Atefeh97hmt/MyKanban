<script setup>

    import { reactive } from 'vue';

    import InputNew from './InputNew.vue';

    /* Variables reactivas: 
        ref para valores sencillos (números, string, bool...) 
        reactive para mas complejos (objetos o arreglos) */
    let boards = reactive([ // lista de objetos con sus propiedades
        {
            id: crypto.randomUUID(),
            name: "TO-DO",
            items: [    // elementos que movemos de un tablero a otro
                /* {
                    id: crypto.randomUUID,
                    title: "Feature de archivos"
                },
                {
                    id: crypto.randomUUID,
                    title: "Resolver bug"
                }, */
            ]
        },
        {
            id: crypto.randomUUID(),
            name: "DOING",
            items: [    // elementos que movemos de un tablero a otro
                /* {
                    id: crypto.randomUUID,
                    title: "Reporte 23"
                },
                {
                    id: crypto.randomUUID,
                    title: "Revisión disposición display"
                }, */
            ]
        },
        {
            id: crypto.randomUUID(),
            name: "DONE",
            items: [    // elementos que movemos de un tablero a otro
                /* {
                    id: crypto.randomUUID,
                    title: "Cambio en files"
                },
                {
                    id: crypto.randomUUID,
                    title: "Resolución bug arrow function pie de página"
                }, */
            ]
        },
    ]);

    
    function handleNewItem(text, board) {
        //console.log(text.value, board.id, board.name);
        board.items.push({
            id: crypto.randomUUID(),
            title: text.value,
        });
    }

    function handleNewBoard() {
        const name = prompt("Nombre del tablero: ");
        if (!!name) {
            boards.push({
                id: crypto.randomUUID(),
                name: name,
                items: []
            });
        }
    }

    // drag and drop:

    function startDrag(event, board, item) {
        
        event.dataTransfer.setData('text/plain', JSON.stringify({boardId: board.id, itemId: item.id}));

    }

    function onDrop(event, destino) {
        
        const { boardId, itemId} = JSON.parse(event.dataTransfer.getData('text/plain'));

        //console.log(`Soltado con los valores: boardId = ${boardId} e itemId = ${itemId}`);

        const originBoard = boards.find((board) => board.id === boardId);
        const originItem = originBoard.items.find((item) => item.id === itemId);


        // error con elementos sin id
        /* console.log(originBoard.name);
        console.log(originItem.title); */

        destino.items.push({ ...originItem });

        originBoard.items = originBoard.items.filter((item) => item !== originItem);

    }


</script>

<template>
    <div class="boards-container">
        <div class="boards">
            <div class="board" v-for="b in boards" :key="b.id" @drop="onDrop($event, b)" @dragover.prevent @dragenter.prevent>
                <div class="board__name centrar-texto">
                    <h2>{{b.name}}</h2> 
                </div>
                <InputNew @on-new-item="(text) => handleNewItem(text, b)" />
                <div class="board__items"> 
                    <div class="item" v-for="i in b.items" :key="i.id" draggable="true" @dragstart="startDrag($event, b, i)">
                        {{i.title}}
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<style scoped>

.boards-container{
    display: flex;
    justify-content: center;
}

    .boards {
        display: flex;
        justify-content: center;
        align-items: center;
        flex-wrap: wrap;
    }

    .board {
        width: 300px;
        min-height: 400px;
        margin:5px;
        background-color: #404d5b;
        border-radius:5px;
    }
    h2 {
    font-size: 20px;
    text-align: center;
    color: #fff;
    margin: 5px;
}
.item{
    background-color: #ffffff36;
    padding: 0rem 0.5rem;
    margin: 1rem 0;
    /* font-weight: bold; */
    color: #d8eeff;
    font-size: 18px;
}
</style>