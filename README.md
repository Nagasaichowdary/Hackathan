# Hackathon-DE
(i) Teams will have to design a combinational/sequential circuit as the solution for the problem statement.
Problem solving steps:
Given the requirement to design combinational and sequential circuits for addressing traffic congestion in metropolitan cities, here's a more detailed breakdown of potential circuits:

Traffic Light Controller:

Combinational logic can be used to determine the optimal timing for traffic signals based on inputs such as traffic flow, pedestrian crossings, and time of day.
Sequential logic can be employed to control the sequence of traffic light changes and to ensure smooth transitions between states.
Traffic Flow Monitoring:

Combinational circuits can process inputs from sensors detecting vehicle presence and count the number of vehicles passing through.
Sequential circuits can be utilized to store and analyze historical traffic data, identifying patterns and predicting future congestion.
Traffic Signal Coordination:

Combinational logic can be used to coordinate signals at intersections, ensuring that green lights are synchronized to facilitate continuous traffic flow.
Sequential logic can help maintain the timing of signal changes and adjust them dynamically based on real-time traffic conditions.
Dynamic Lane Management:

Combinational circuits can control lane direction indicators based on inputs such as traffic volume and congestion levels.
Sequential circuits can manage the switching of lanes, ensuring smooth transitions and preventing collisions.
Route Optimization:

Combinational logic can be employed to calculate optimal routes based on inputs such as traffic data, road conditions, and distance.
Sequential logic can store and update route information, guiding drivers through the most efficient paths.
Vehicle Priority Systems:

Combinational circuits can determine priority for vehicles such as emergency vehicles, public transport, and high-occupancy vehicles based on predefined criteria.
Sequential circuits can manage the allocation of priority lanes and signals, ensuring that priority vehicles can move smoothly through traffic.
Traffic Data Analysis:

Combinational logic circuits can process raw traffic data collected from sensors and detectors, performing tasks such as filtering, sorting, and aggregating.
Sequential circuits can analyze the processed data over time, identifying trends, patterns, and areas of congestion.
Adaptive Traffic Control:

Combinational logic can be used to design algorithms for adaptive traffic control, which dynamically adjust traffic signal timings and lane configurations based on real-time inputs.
Sequential logic circuits can implement learning mechanisms that adapt the system's behavior over time based on observed traffic patterns and feedback.
(ii) Final and intermediate outputs of the code/RTL Schematic implementation will be observed and will be given high weightage.

Verilog Code:
```
module traffic(
    input clk,             // Clock input
    input reset,           // Reset input
    input vehicle_NS,      // Vehicle presence in North-South direction
    input vehicle_EW,      // Vehicle presence in East-West direction
    input pedestrian_NS,   // Pedestrian crossing in North-South direction
    input pedestrian_EW,   // Pedestrian crossing in East-West direction
    output reg green_NS,   // North-South green light
    output reg yellow_NS,  // North-South yellow light
    output reg red_NS,     // North-South red light
    output reg green_EW,   // East-West green light
    output reg yellow_EW,  // East-West yellow light
    output reg red_EW      // East-West red light
);

parameter NS_GREEN = 2'b00;
parameter NS_YELLOW = 2'b01;
parameter EW_GREEN = 2'b10;
parameter EW_YELLOW = 2'b11;

// Define signal timing parameters
parameter GREEN_DURATION = 10;  // Duration for green light in clock cycles
parameter YELLOW_DURATION = 2;  // Duration for yellow light in clock cycles

// Define internal signals for FSM
reg [1:0] state;
reg [3:0] timer;

always @(posedge clk or posedge reset) begin
    if (reset) begin
        state <= NS_GREEN;  // Initialize FSM to North-South green
        timer <= 0;
    end else begin
        // FSM state transitions
        case (state)
            NS_GREEN: begin
                if (timer >= GREEN_DURATION) begin
                    state <= NS_YELLOW;  // Transition to North-South yellow
                    timer <= 0;
                end else if (vehicle_EW || pedestrian_NS) begin
                    state <= EW_GREEN;  // Transition to East-West green
                    timer <= 0;
                end else begin
                    timer <= timer + 1;
                end
            end
            NS_YELLOW: begin
                if (timer >= YELLOW_DURATION) begin
                    state <= EW_GREEN;  // Transition to East-West green
                    timer <= 0;
                end else begin
                    timer <= timer + 1;
                end
            end
            EW_GREEN: begin
                if (timer >= GREEN_DURATION) begin
                    state <= EW_YELLOW;  // Transition to East-West yellow
                    timer <= 0;
                end else if (vehicle_NS || pedestrian_EW) begin
                    state <= NS_GREEN;  // Transition to North-South green
                    timer <= 0;
                end else begin
                    timer <= timer + 1;
                end
            end
            EW_YELLOW: begin
                if (timer >= YELLOW_DURATION) begin
                    state <= NS_GREEN;  // Transition to North-South green
                    timer <= 0;
                end else begin
                    timer <= timer + 1;
                end
            end
            default: state <= NS_GREEN;  // Default to North-South green
        endcase
    end
end

// Output control based on FSM state
always @* begin
    case (state)
        NS_GREEN: begin
            green_NS = 1'b1;
            yellow_NS = 1'b0;
            red_NS = 1'b0;
            green_EW = 1'b0;
            yellow_EW = 1'b0;
            red_EW = 1'b1;
        end
        NS_YELLOW: begin
            green_NS = 1'b0;
            yellow_NS = 1'b1;
            red_NS = 1'b0;
            green_EW = 1'b0;
            yellow_EW = 1'b0;
            red_EW = 1'b1;
        end
        EW_GREEN: begin
            green_NS = 1'b0;
            yellow_NS = 1'b0;
            red_NS = 1'b1;
            green_EW = 1'b1;
            yellow_EW = 1'b0;
            red_EW = 1'b0;
        end
        EW_YELLOW: begin
            green_NS = 1'b0;
            yellow_NS = 1'b0;
            red_NS = 1'b1;
            green_EW = 1'b0;
            yellow_EW = 1'b1;
            red_EW = 1'b0;
        end
        default: begin
            green_NS = 1'b1;
            yellow_NS = 1'b0;
            red_NS = 1'b0;
            green_EW = 1'b0;
            yellow_EW = 1'b0;
            red_EW = 1'b1;
        end
    endcase
end

endmodule
```
RTL View:
![image](https://github.com/Nagasaichowdary/Hackathan/assets/155174528/6b97ecc6-a623-4c6d-82a0-a1411ab50b72)

Result:
Successfully developed the solution for Traffic Congestion in Metro Cities using verilog code.
