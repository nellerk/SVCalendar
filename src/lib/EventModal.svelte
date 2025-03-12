<script>
    import { createEventDispatcher } from "svelte";
    import { onMount } from "svelte";

    let show = false;
    let eventDate = "";
    let eventTitle = "";
    let eventDescription = "";

    const dispatch = createEventDispatcher();

    function openModal(date, title = "", description = "") {
        eventDate = date;
        eventTitle = title;
        eventDescription = description;
        show = true;
    };

    function closeModal() {
        show = false;
    };

    function saveEvent() {
        dispatch("save", {date: eventDate, title: eventTitle, description: eventDescription});
        closeModal();
    };

    function deleteEvent() {
        dispatch("delete", eventDate);
    };

    export { openModal };
</script>


{#if show}
    <div class="modal-overlay">
        <div class="modal">
            <h2>{eventDate} esemény kezelése</h2>
            <label for="text"> Esemény neve:</label>
            <input type="text" bind:value={eventTitle} placeholder="Pl. Meeting" />

            <label for="description">Leírás:</label>
            <textarea bind:value={eventDescription} placeholder="Pl. Projekt ertekezlet"></textarea>

            <div class="modal-buttons">
                <button on:click={saveEvent} class="btn-save">Mentés</button>
                <button on:click={deleteEvent} class="btn-delete">Törlés</button>
                <button on:click={closeModal} class="btn-close">Mégse</button>
            </div>
        </div>
    </div>
{/if}

<style>
    .modal-overlay {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, 0.5);
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .modal {
        background: white;
        padding: 20px;
        border-radius: 8px;
        width: 300px;
    }

    .modal-buttons {
        margin-top: 10px;
        display: flex;
        justify-content: space-between;
    }

    .btn-save {
        background: green;
        color: white;
        padding: 5px 10px;
        border: none;
        cursor: pointer;
    }

    .btn-delete {
        background: red;
        color: white;
        padding: 5px 10px;
        border: none;
        cursor: pointer;
    }

    .btn-close {
        background: gray;
        color: white;
        padding: 5px 10px;
        border: none;
        cursor: pointer;
    }
</style>