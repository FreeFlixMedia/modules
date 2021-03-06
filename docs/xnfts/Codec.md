

### Codec

Once we have defined our messages, we need to describe to our encoder how they should be stored as bytes. To do this we edit the file located at `./xnfts/internal/types/codec.go`. By describing our types as follows they will work with our encoding library:

```go=
package types

import (
	"github.com/cosmos/cosmos-sdk/codec"
	cdctypes "github.com/cosmos/cosmos-sdk/codec/types"
	sdk "github.com/cosmos/cosmos-sdk/types"
	channel "github.com/cosmos/cosmos-sdk/x/ibc/04-channel"
	commitmenttypes "github.com/cosmos/cosmos-sdk/x/ibc/23-commitment/types"
)

var (
	amino     = codec.New()
	ModuleCdc = codec.NewHybridCodec(amino, cdctypes.NewInterfaceRegistry())
)

// RegisterCodec registers the IBC transfer types
func RegisterCodec(cdc *codec.Codec) {
	cdc.RegisterConcrete(MsgXNFTTransfer{}, "ibc/xnfts/MsgXNFTTransfer", nil)
	cdc.RegisterConcrete(MsgPayLicensingFee{}, "ibc/xnft/MsgPayLicensingFee", nil)
	
	cdc.RegisterConcrete(BaseNFTPacket{}, "ibc/xnfts/BaseNFTPacket", nil)
	cdc.RegisterConcrete(PacketPayLicensingFeeAndNFTTransfer{}, "ibc/xnft/PacketPayLicensingFeeAndNFTTransfer", nil)
	
	cdc.RegisterInterface((*XNFTs)(nil), nil)
}

func RegisterInterfaces(registry cdctypes.InterfaceRegistry) {
	registry.RegisterImplementations((*sdk.Msg)(nil), &MsgXNFTTransfer{})
}

func init() {
	RegisterCodec(amino)
	channel.RegisterCodec(amino)
	commitmenttypes.RegisterCodec(amino)
	amino.Seal()
}


```