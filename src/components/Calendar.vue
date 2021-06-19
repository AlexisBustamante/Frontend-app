<template>
<v-container>
    <v-card dark>

        <v-sheet>
            <v-calendar :type="type" :now="today" value="2021-06-18" :events="events" @click:event="showEvent">

            </v-calendar>
            <v-menu v-model="selectedOpen" :activator="selectedElement" offset-x>
                <v-card>
                    <v-toolbar :color="selectedEvent.color" dark>
                        <v-btn icon>
                            <v-icon>mdi-pencil</v-icon>
                        </v-btn>
                        <v-toolbar-title v-html="selectedEvent.name">
                        </v-toolbar-title>
                        <v-divider></v-divider>

                    </v-toolbar>
                    <v-card-text>
                        <span v-html="selectedEvent.details"></span>
                    </v-card-text>
                    <v-card-actions>
                        <v-btn color="success" @click="selectedOpen=false">
                            Cerrar
                        </v-btn>
                    </v-card-actions>
                </v-card>
            </v-menu>

        </v-sheet>
    </v-card>
</v-container>
</template>

<script>
export default {
    name: 'Calendar',
    data: () => ({
        type: 'month',
        today: new Date().toISOString().substr(0, 10),
        focus: new Date().toISOString().substr(0, 10),
        selectedEvent: {},
        selectedElement: null,
        selectedOpen: false,
        typeToLabel: {
            month: 'Month',
            week: 'Week',
            day: 'Day',
            '4day': '4 Days',
        },
        events: [{
                name: "meeting 01",
                start: "2021-06-18 7:00",
                end: "2021-06-18 10:00",
                color: 'primary',
                details: 'Visita a realizar pronto'
            },
            {
                name: "meeting 01",
                start: "2021-06-08 7:00",
                end: "2021-06-09 10:00",
                color: 'primary',
                details: 'Visita a realizar pronto'
            },
            {
                name: "meeting 02",
                start: "2021-06-18 8:30",
                end: "2021-06-18 11:00",
                color: "yellow",
                details: 'Visita a realizar pronto'

            },
            {
                name: "meeting 03",
                start: "2021-06-18 8:00",
                end: "2021-06-18 11:40",
                color: "purple",
                details: 'Visita a realizar pronto'

            },
            {
                name: "meeting 04            ",
                start: "2021-06-13 12:00",
                end: "2021-06-19 13:00",
                color: "success",
                details: 'Visita a realizar pronto'
            },
            {
                name: "meeting 03",
                start: "2021-06-15 8:00",
                end: "2021-06-18 11:40",
                color: "purple",
                details: 'Visita a realizar pronto'

            },
        ]

    }),
    mounted() {

    },
    methods: {
        showEvent({
            nativeEvent,
            event
        }) {
            const open = () => {
                this.selectedEvent = event;
                this.selectedElement = nativeEvent.target;
                this.selectedOpen = true;
            };

            if (this.selectedOpen) {
                this.selectedOpen = false;
            } else {
                open();
            }

            nativeEvent.stopPropagation();

        }
    }

}
</script>
